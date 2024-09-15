<template>
  <div>
    <h1>Create User</h1>
    <input v-model="userName" placeholder="Name" />
    <input v-model="userEmail" placeholder="Email" />
    <button @click="createUser">Create User</button>
  </div>
</template>

<script setup lang="ts">
import axios from 'axios'
import { ref } from 'vue'

const userId = ref('')
const userName = ref('')
const userEmail = ref('')

const users = ref<{ userId: string; userName: string; userEmail: string }[]>([])

const createUser = async () => {
  try {
    if (!userName.value || !userEmail.value) {
      alert('Please fill all fields')
      return
    }

    userId.value = Math.random().toString(36).substr(2, 9)

    users.value.push({
      userId: userId.value,
      userName: userName.value,
      userEmail: userEmail.value
    })

    await updateAPIFile()

    userName.value = ''
    userEmail.value = ''
  } catch (err) {
    console.error(err)
  }
}

const updateAPIFile = async () => {
  const filePath = import.meta.env.VITE_FILE_PATH
  const repoOwner = import.meta.env.VITE_REPO_OWNER
  const repoName = import.meta.env.VITE_REPO_NAME
  const accessToken = import.meta.env.VITE_ACCESS_TOKEN
  const branch = import.meta.env.VITE_BRANCH_NAME

  try {
    let fileSHA = ''
    let fileContent = ''

    // Lặp lại nếu có xung đột
    while (true) {
      try {
        // 1. Lấy thông tin về file từ GitHub
        const getFileRes = await axios.get(
          `https://api.github.com/repos/${repoOwner}/${repoName}/contents/${filePath}?ref=${branch}`,
          {
            headers: {
              Authorization: `token ${accessToken}`,
              Accept: 'application/vnd.github.v3+json'
            }
          }
        )

        fileSHA = getFileRes.data.sha
        fileContent = atob(getFileRes.data.content)
        console.log('File content:', fileContent) // Kiểm tra nội dung file

        // 2. Xử lý nội dung JSON
        let apiData: { users: { userId: string; userName: string; userEmail: string }[] } = {
          users: []
        }
        if (fileContent.trim()) {
          try {
            apiData = JSON.parse(fileContent)
          } catch (e) {
            console.error('Error parsing JSON:', e)
            apiData = { users: [] }
          }
        }
        apiData.users = [...apiData.users, ...users.value]

        const updatedContent = btoa(JSON.stringify(apiData, null, 2))

        await axios.put(
          `https://api.github.com/repos/${repoOwner}/${repoName}/contents/${filePath}`,
          {
            message: 'Update user.json',
            content: updatedContent,
            sha: fileSHA,
            branch
          },
          {
            headers: {
              Authorization: `token ${accessToken}`,
              Accept: 'application/vnd.github.v3+json'
            }
          }
        )
        console.log('Update user.json success')
        break
      } catch (err) {
        if (err.response && err.response.status === 409) {
          console.log('Conflict detected. Retrying...')
        } else {
          console.error('Error updating user.json:', err)
          break
        }
      }
    }
  } catch (err) {
    console.error('Error:', err)
  }
}
</script>
