# Setting up a dev container for Rust

* Primary author: [Siddhant Saxena](https://github.com/sisaxena42)

* Reviewer: [Arden Feldt](https://github.com/Arden-Feldt)

# Welcome!

As we progress through our undergraduate CS tenure, we are surely to become better and (hopefully) more efficent software engineers. There are many niche and interesting programming languages out there that have many purposes. As we have set up a dev container for python and ran mkdocs for a static website, we will now setup a dev container for rust, along with its package manager 'cargo', and setup a simple compiled code to run "Hello Comp 423".

## Prerequistes
Before we begin creating a dev container for Rust, we need to make sure we have some preliminary steps taken before implementing the container and producing our program.

* [Github Account](https://github.com) / [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) installed
* [Visual Studio Code](https://code.visualstudio.com) / Required extenstions (we will talk more later)
* [Docker](https://www.docker.com/products/docker-desktop/) Installed


## Part 1: Project Setup: Creating the initial Repository
### Step 1: Using git to create a local directory 
1. Open terminal or command prompt (cmd.exe)
2. Create a new local directory for your project using the following command. (You can make this directory anywhere if you do not want it's default location).

    ```
    mkdir rust-setup-proj 
    cd rust-setup-proj
    ```
3. Now we initialize a new git repository by running:
```
git init
```
4. Create a README.md file:
```
echo "# Rust Program" > README.md
git add README.md
git commit -m "Initial commit with README"
```

### Step 2: Create a remote repository on Github 
1. Login to your Github account and go to the [new repository page](https://github.com/new).
2. Fill in the following details:
    * Repository Name: `rust-program`
    * Description: `a simple rust program`
    * Visibility: `public`
3. Make sure that you have unchecked the initialization with a README, .gitignore, and lisence.
4. Create the repository and clone it to your machine via:
```
git remote add origin https://github.com/<your-username>/rust-program.git
```
5. Your default branch should be named "main", if it isnt, you can change it by running `git branch -M main`. If you do, you'll have to re-add and re-commmit.
6. Now you can push to your remote repository by running:
```
git push --set-upstream origin main
```

## Part 2: Seting up the actual Development Container/Environment
### Step 1: Setting up Configuration for 

1. Open the `rust-program` directory in VS code and make sure you have the Dev Containers extention on VS code as well.
2. Create a `.devcontainer` directory (folder) in the root of your project folder.
3. Create a `devcontainer.json` file inside the `.devcontainer` directory. This file actually holds the configuration for the dev container.
4. Copy the below config file data and put into your `devcontainer.json` file:

???+ info inline end "Anatomy of a configuration file"
    ??? note "name"
        `name`: The descriptive name for the dev container
    ??? note "image"
        `image`: This is the docker image to use in the container. In this case, it is rust.
    ??? note "customizations"
        `customizations`: This holds useful configurations to VS code that you can typically find on the marketplace. In our case, we will install any VS code extentions we need here for our project.
    ??? note "postCreateCommand"
        `postCreateCommand`: A command that runs after the container is formed, usually used to install any requirements from the `requirements.txt` file.
```
{
  "name": "Rust Program",
  "image": "mcr.microsoft.com/vscode/devcontainers/rust:latest",
  "customizations": {
    "vscode": {
      "settings": {},
      "extensions": ["rust-lang.rust-analyzer"]
    }
  }
}
```
5. Now you are ready to open the project in the container. In order to do this, push down on `command(or control) + shift + p`, type "reopen in container" on the search bar and click on that. This process may take some time.

???+ warning
    Make sure you have docker open and running prior to this step. You can also verify if you have rust installed after opening the container by running `rustc --version` to prove a recent version. Example output: `rustc 1.83.0 (90b35a623 2024-11-26)`.
## Part 3: Making your first Rust project
### Step 1: Rust this 
???+ abstract "Rust and Cargo"
    Rust is a special language that has the ability to minimize bug priority as its own compiler acts as a gatekeeper to ensure that the developers only focus of building code. This language mkaes it easier to control low-level details with minimla hassle. This is done through rust's built-in compiler `rustc` and its package manager `cargo` which primarily handles dependencies, building and compiler overlooking.
1. Rust programs libraries/executable files are called `crates`. These can be handled with ease due to cargo's functionality. Let's start to create our first Rust program by initializing our first cargo package to hold our crate and manifest.
???+ note inline end
    `manifest` just means all the metadata needed to compile the crate.
    Additionally, the `--vcs none` tag just denies cargo to initialize a git repository.

In order to create our first package, run the following code:
```
cargo new hello_comp423 --vcs none
```
2. This should create 3 new things. A file called `Cargo.toml`, a directory called `src`, and a file in that directory called `main.rs`. The `.rs` extention is the proper extention for rust files. This is the file that contains the source code. 
For now, navigate to the `main.rs` file and replace the contents to:
```
fn main() {
    println!("Hello, COMP423!");
}
```
### Step 2: Compiler
???+ warning "Careful"
    Before we compile the code we 'crated'(cs humor), we want to make sure we are in the correct working directory. We can do this by running:
    ```
    cd hello_comp423
    ```
1. Cargo has generated us a `crate`. We can now compile and run this file using cargo's commands: `cargo build` and `cargo run`. However, these do different things. 
```
cargo build
```
Running this code will cause cargo to compile the `.rs` file, in our case the `main.rs` file, and places the resulting binary in `target/debug`. This means that when we run the binary, the code will take less to compile as it will be run in debug mode. This is used primarily for development. However, if the `.rs` file is ready, then we run:
```
cargo build --release
```
This compiles and places the binary in `target/release`. This means that the code will take longer to compile but it will run quicker. 

Think of `cargo build` as the `gcc` command from 211 but with only two attached flags, `build` and `build --release`.

### Step 3: Running
#### 1. You can manually run the execuatble with:


???+ example inline end "Alert"
    This should show "Hello, COMP423!" on your console


```
    ./target/debug/hello_comp423
```
or 
    ```
    ./target/release/hello_comp423
    ```
depedning on what compile you performed. 



#### 2. Additonally, you can combine both the compiling and running with one cargo command:
```
cargo run
```
Note: this command may not provide any feedback on the compiling step if no chnages were made to that file.

???+ bug "Cargo.lock?"
    After running the excecutable file by any means, cargo will create a `Cargo.lock` file. This file just contains information about the dependencies but in this case, there wont be much in there.

# Congratulations!
## Victory!
 You have sucessfully created your first rust program in a dev container! You can now push you changes to your github repositiory with the following commands:
 ```
 git add .
 git commit -m "im better at this rust than the other one fs"
 ```

 and finally 
 ```
 git push origin
 ```