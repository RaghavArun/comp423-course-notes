# Setting up a dev container for Go.

* Primary author: [Raghav Arun](https://https://github.com/RaghavArun/)
* Reviewer: [Nicolas Asanov](https://github.com/naasanov)

## Prerequisites: 

1. **A GitHub account:** If you don’t have one yet, sign up at [GitHub](https://github.com/).
2. **Git installed:** [Install Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) if you don’t already have it.
3. **Visual Studio Code (VS Code):** Download and install from [here](https://code.visualstudio.com/).
4. **Docker Installed:** Install Docker Desktop from [here](https://www.docker.com/products/docker-desktop/).
5. **Dev Containers Extension for VS Code:** If it's not installed already, navigate to the "Extensions" tab in VS Code, search "Dev Containers", and install the Microsoft option.
6. **Command-Line Basics:** Be familiar with basic commands such as `mkdir` and `cd`.

## Step 1: Create a Local Directory and Initialize Git

### Create the Project Directory

1. Open your terminal or command prompt. 
2. Create a directory for your project. 

Make sure you know where your directory is going. An easy way to do this is to `cd` to your home directory and create this inside there. 

For Linux/Mac:
```bash
$ cd 
```

For Windows:
```bash
$ cd %HOMEPATH%
```

Now that you have `cd`'ed to your home directory (or a directory where you want to make this project directory in), go ahead and make the project directory using the following commands:

```bash
mkdir go-tutorial 
cd go-tutorial
``` 

### Initialize Git repository and write a README file

1. Initialize a new Git repository with this command:

    ```bash
    git init
    ```

    ??? info "What happens when I run the `init` subcommand?"
    Running `git init` intializes a folder as a new, empty `git` repository.

2. Create a README file:

    ```bash
    echo "# Go Tutorial following instructions from https://raghavarun.github.io/comp423-course-notes/tutorials/go-setup/" > README.md
    git add README.md
    git commit -m "Initial commit with README"
    ```

## Step 2: Create a Remote Repository on GitHub and Link your Local Repository

### Create Remote Repository

1. Log in to your GitHub account and navigate to the [Create a New Repository](https://github.com/new) page.

2. Fill in the details as follows: 
* **Repository name:** `go-tutorial`
* **Description:** "Simple 'Hello World' program in Go using Dev Container."
* **Visibility:** Public

3. Do not initialize the repository with a README (you already wrote one), .gitignore, or license.

4. Click **Create Repository**.

### Link your Local Repository to GitHub

1. Add the GitHub repository as a remote:
    ```bash
    git remote add origin https://github.com/your-username/go-tutorial.git
    ```

    Replace `your-username` with your GitHub username.

2. Check your default branch name with the subcommand `git branch`. If it isn't `main`, rename it to `main` with the following command: 

    ```bash
    git branch -M main
    ```

3. Push your local commits to the GitHub repository: 

    ```bash
    git push --set-upstream origin main
    ```

??? info "What does the --set -upstream flag do?"
`git push --set-upstream origin main`: This command pushes the main branch to the remote repository origin. The `--set-upstream` flag sets up the main branch to track the remote branch, meaning future pushes and pulls can be done without specifying the branch name and just writing `git push origin` when working on your local main branch. This long flag has a corresponding `-u` short flag. (From [UNC COMP 423 MKDocs Tutorial] (https://comp423-25s.github.io/resources/MkDocs/tutorial/#step-3-link-your-local-repository-to-github)) 

4. In your web browser, refreshing your GitHub repository will show that the same commit you made locally has now been pushed to the remote repository. 

## Step 3: Setting Up the Dev Container

### Add Dev Container Configuration

1. Open the `go-tutorial` directory in VS Code. You can do this by simply clicking File > Open Folder and opening the directory.

2. Inside your `go-tutorial` directory, create a `.devcontainer` folder: 

    ```bash
    mkdir .devcontainer 
    cd .devcontainer
    ```

    You can also create the folder by clicking the **new folder** symbol that appears in the SOURCE CONTROL section of your VS Code interface. 

3. Create a file named `devcontainer.json` in the `.devcontainer` folder. This will be your configuration file. We will be using the following configuration: 

    ```json title='devcontainer.json'
    {
    "name": "Go Tutorial",
    "image": "mcr.microsoft.com/devcontainers/go:1.22",
    "customizations": {
        "vscode": {
        "settings": {
            "go.gopath": "/go"
        },
        "extensions": [
            "golang.go"
        ]
        }
    }
    }
    ```

    * The **Docker Image** for this dev container is the Microsoft prebuilt container image for Go development. It includes Go version `1.22` and common Go dependencies. 
    * We also include the **Go Extension** for VS Code, which provides helpful features like highlighting syntax, code autocompletion, and hover information.

### Reopen the project in a VS Code Dev Container

Reopen the project in the dev container we just configuered by pressing `Ctrl+Shift+P` (Windows) or `Cmd+Shift+P` (Mac), typing "Dev Containers: Reopen in Container," and selecting the option. VS Code will build the container and reopen your project inside it. It may take a few minutes for the image to be downloaded. 

###  Verifying Go Installation:

Close the terminal tab you were working and open a terminal pane within VS Code (you can use ```Ctrl+` ``` to show the terminal pane).

Now, you can check to ensure your dev container is running the correct Go version (`1.22`) by running the following command:

```bash
go version
```

## Step 4: Using Go Commands

### Initializing a Go Module

Before we can run code with Go, we need to enable dependency tracking for our code by initializing a go module and creating a go.mod file. We can do this by running the `go mod init` command with the name of the module our code will be in:

```bash
go mod init go-tutorial
```

### Writing a Simple Go Program

Create a file named `hello.go` in the root of your project:

```Go title='hello.go'
package main 

import "fmt"

func main() {
    fmt.Println("Hello COMP423")
}
```

### Running a Go Program

Use the `go run` command in the terminal to execute the program:

```bash
go run hello.go
```
* Output:
    `Hello COMP423`

### Building a Go Program

1. Use the `go build` command to create an executable binary: 

    ```bash
    go build -o runhello
    ```
    * This generates an executable file named `runhello` in the current directory. 

2. Run the binary directly:

    ```bash
    ./runhello
    ```
    * Output:
        `Hello COMP423`

### Using `run` vs `build`

`go run` compiles and runs the code in a single step, making it ideal for development and testing. You can easily run your code without elaving behind a compiled binary file. 

`go build` generates an executable binary that can be executed without Go installed. This is similar to using the `gcc` command as we did in COMP 211 to compile C code into executables. This binary is portable and can be shared or deployed. You can delete the program easily by running `rm programname`.

Speaking of which, let's remove the binary we built:

```bash
rm runhello
```

## Step 5: Committing and Pushing Changes to Remote Repository

Now that we have created and run code in Go, we will conclude our tutorial by pushing our changes to the remote repository we created. We'll just commit and push once more:

```bash
git add . # adds all files we made
git commit -m "Wrote Hello World code and ran it"
```

```bash
git push --set-upstream origin main
```