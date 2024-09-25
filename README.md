# Welcome to DevOpsCloud Junction, Empowering Innovation Through DevOps, Cloud Native, AI, and Emerging Technologies!"

- [DevOpsCloudJunction](https://devopscloudjunction.com/)

To Contribute, please follow the below instructions.

### 1. Requirements

- [Git](https://git-scm.com/) — latest source release
- [Node.js](https://nodejs.org/) — latest LTS version or newer
- [Hugo] (https://gohugo.io/) 


```bash
git clone https://github.com/devopscloudjunction/website.git website && cd website
```

### 2. Install dependencies

```bash
npm install
```

Install Hugo

```bash
brew install hugo 
```
Check Hugo Version 

```bash
hugo version
```

### 3. Start development server

```bash
npm run start
```

or you can use hugo for local server at http://localhost:1313 using below command

```bash
hugo server 
```


### Steps to make blog addition 

1. Create a new branch out of 'main' branch using below git command and work in that branch

```bash 
git checkout -b sample-blog
```
2. Navigate to content/en/blog and create a directory for your blog. for example, 'test-blog'

3. Copy sample index.md file from content/en/blog/say-hello-to-devopscloudjunction directory and dcj.jpeg image inside your created directory 'test-blog'

or you can use below command to create a new blog dir and index.md

```bash 
hugo new blog/<blog-name>/index.yaml
```

here the only variable should be <blog-name> for example - my-test-blog

4. Edit the index.md to add your title, description, images or tags and so on. 

Note: Image is must add, if you have a relevant image, copy in the same directory or use the default dcj.jpeg image in the images field

5. Start your content writing in the index.md using markdown syntax, you can refer to this link for tips on markdown (https://www.markdownguide.org/cheat-sheet/)

Tip: You can also bash or yaml blocks depending on requirements.
(https://www.thecodebuzz.com/highlight-bash-shell-code-in-markdown-readme-md-wiki-files/) 
example checkout the existing index.md in (content/en/blog/kube-autoscaler/index.md)

6. Once you have complete the blog, run hugo server to start the local copy of the website and test the changes on (http://localhost:1313), the blog should appear along with other blogs inside the (https://devopscloudjunction.com/blog/) upon clicking 'Get Started' on homepage

To build your site, cd into your project directory (where) and run

```bash 
hugo server
```

7. If the changes looks good, commit the changes with meaningful commit message and initate a Pull request, add reviewers. you can also check the Deploy Preview version of website for your commited changes on Git Pull Request area

8. Once changes are approved by reviewers, the branch will be merged to 'main'