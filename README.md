
# Welcome to **DevOpsCloud Junction**  
*Empowering Innovation Through DevOps, Cloud Native, AI, and Emerging Technologies!*

🌐 **[DevOpsCloudJunction.com](https://devopscloudjunction.com/)**

---

## How to Contribute

Ready to contribute? Follow the steps below to get started!

###  Prerequisites

Make sure you have the following installed:

- **[Git](https://git-scm.com/)** — Latest version
- **[Node.js](https://nodejs.org/)** — Latest LTS version or newer
- **[Hugo](https://gohugo.io/)** — A framework for building websites

---

### Fork the repository
Go to the [repository](https://github.com/devopscloudjunction/website) and click the "Fork" button on the `top right` to create a copy in your GitHub account.

### Clone the Repository

After forking, use the following command to clone your forked repository:
```bash
git clone https://github.com/devopscloudjunction/website.git website && cd website
```

---

### Install Dependencies

Next, install the necessary Node.js packages:

```bash
npm install
```

And install **Hugo**:

```bash
brew install hugo
```

Verify the Hugo installation:

```bash
hugo version
```

---

### Start the Development Server

You can start the development server in one of two ways:

- **Using npm**:

    ```bash
    npm run start
    ```

- **Using Hugo**:

    ```bash
    hugo server
    ```

Visit [http://localhost:1313](http://localhost:1313) to view the local site.

---

## Blog Contribution Guide

Follow these steps to add a new blog post:

### Create a New Branch

Create a new branch from the `main` branch for your blog post:

```bash
git checkout -b sample-blog
```

### Create Blog Directory

Run the below command from `root` directory to get ready with your blog layout.

```bash
hugo new blog/<blog-name>/index.md
```

> Replace `<blog-name>` with a meaningful name related to your blog topic. 

Alternatively, you can manually create a folder and copy the `index.md` from an existing blog post, like `say-hello-to-devopscloudjunction`.

### Edit Your Blog Post

Edit the `index.md` file and update the fields:

- **Title**: Add your blog title.
- **Description**: Provide a brief summary of your blog.
- **Image**: Include an image. Use either a relevant image or the default `dcj.jpeg` located at `images/dcj.jpeg`.
- **Weight**: Keep the weight as value 100 for your blogs, the redordering will be based on the date

For **Markdown tips**, check out the [Markdown Cheat Sheet](https://www.markdownguide.org/cheat-sheet/).

For **code highlighting**, refer to [The Code Buzz](https://www.thecodebuzz.com/highlight-bash-shell-code-in-markdown-readme-md-wiki-files/).

### Preview Your Blog Locally

Run the Hugo server to preview your blog locally:

```bash
hugo server
```

Check [http://localhost:1313](http://localhost:1313) to ensure everything looks good.

### Commit and Open a Pull Request

Once satisfied with your changes:

1. **Commit the changes**:

    ```bash
    git add .
    git commit -m "Added blog: <your-blog-title>"
    ```

2. **Push the branch** and create a pull request:

    ```bash
    git push origin sample-blog
    ```

3. Add reviewers to your PR, and you can preview the changes through Deploy Preview.

### Merge the Branch

Once approved, your branch will be merged into the `main` branch and your blog will go live on the website.

---

### Notes:

- Always include an image with your blog. If unsure, use the default image `dcj.jpeg`.
- Follow proper markdown formatting and code block highlighting when necessary.
