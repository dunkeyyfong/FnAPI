# FnAPI Using GitHub Pages to API

## Overview

FnAPI is a project that utilizes GitHub Pages to host and serve a JSON-based API. This API allows you to manage user data and interact with the data through a web interface.

## Features

- **Dynamic User Management**: Create, read, and update user data using GitHub Pages.
- **Easy Deployment**: Leveraging GitHub Pages for API hosting and data storage.
- **Simple Integration**: Easily integrate with other services and applications.

## Setup

### Prerequisites

- **Git**: Ensure that Git is installed on your machine.
- **GitHub Account**: You need a GitHub account to host the repository and use GitHub Pages.

### Steps to Set Up

1. **Clone the Repository**

   ```bash
   git clone https://github.com/yourusername/FnAPI.git
   cd FnAPI
   ```

2. **Set Up Environment Variables**

   Create a `.env` file in the root directory of the project with the following content:

   ```env
   VITE_FILE_PATH=user.json
   VITE_REPO_OWNER=yourusername
   VITE_REPO_NAME=yourrepository
   VITE_ACCESS_TOKEN=your_github_access_token
   VITE_BRANCH_NAME=main
   ```

3. **Configure GitHub Pages**

   - Go to the GitHub repository page.
   - Navigate to **Settings** > **Pages**.
   - Select the branch (`main` or `gh-pages`) and folder (usually `/root`).
   - Save the settings.

4. **Run the Application**

   If you are using a local development environment, start your application. For example, if you're using Vue:

   ```bash
   npm install
   npm run serve
   ```

5. **Deploy Changes**

   Push your changes to GitHub:

   ```bash
   git add .
   git commit -m "Initial setup for FnAPI"
   git push origin main
   ```

## Usage

### Creating a User

To create a new user, simply use the web interface provided by the GitHub Pages site. Fill in the user details and click "Create User."

### Updating the API File

The API file (`user.json`) will be updated automatically whenever a new user is created or existing data is modified.

[Example API](https://dunkeyyfong.github.io/user.json)

## Troubleshooting

### Unauthorized Error

If you encounter a 401 Unauthorized error:

- Check that your GitHub access token is correct and has the necessary permissions.
- Ensure that the `.env` file is correctly configured.

### Conflict Error

If you encounter a 409 Conflict error:

- Make sure that you have resolved any potential conflicts in the `user.json` file before pushing changes.
- Check if there are concurrent modifications and resolve them.

## Contributing

Feel free to contribute to the project by submitting issues or pull requests. For more details, check out the [contributing guide](CONTRIBUTING.md).

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
