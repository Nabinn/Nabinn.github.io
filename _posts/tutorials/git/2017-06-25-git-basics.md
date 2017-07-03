---
layout: post
title: Basics of Git
subtitle: A tutorial for beginners
comments: true
category: tutorial
subcategory: git
image: /img/git_logo.png
show-avatar: false
---
Git is a version control system (VCS) for tracking changes in files and coordinating work on those files among multiple people. It is primarily used for source code management in software development, but it can be used to keep track of changes in any set of files.It was created by Linus Torvalds in 2005 for development of the Linux kernel, with other kernel developers contributing to its initial development.

## What can git do?
- keep track of changes to your code
- Synchronize code between different people
- Test changes to code without losing the original
- Revert back to an older version of the code 


## Some terminologies
---
### Repository or "repo":
A directory or storage space where your projects can live.  It can be a local folder on your computer, or it can be a storage space on GitHub or another online host. 

### Fork and Pull request:
Forking = creating your own personal copy of someone else's project. Forks act as a sort of bridge between the original repository and your personal copy. You can submit Pull Requests to help make other people’s projects better by offering your changes up to the original project.

Steps for submitting pull requests: 
	<br>1. Fork a github repository to your own github account.
	<br>2. Clone the forked repository to your local computer.
	<br>3. Make changes in the local repository and commit them.
	<br>4. Push the local repository to your github account.
	<br>5. Make a pull request through github account.

## Commands
---
### 1. init
- The git init command creates a new Git repository. It can be used to convert an existing, unversioned project to a Git repository or initialize a new, empty repository. Most other Git commands are not available outside of an initialized repository, so this is usually the first command you'll run in a new project.
- This command creates an empty Git repository - basically a .git directory with subdirectories for objects , refs/heads , refs/tags , and template files. An initial HEAD file that references the HEAD of the master branch is also created. Running git init in an existing repository is safe.

 To transform the current directory into a git repository, use the following command:
```
git init
```
 To create a git repository in a specified directory, use the following command:
```
git init [directory]
```
### 2. clone
- makes a copy of the remote repository on your computer. 
```
git clone [url]
```


{% highlight java linenos %}
public class HelloWorld{
	public static void main(String[] args){
		System.out.println("Hello World");
}
	public void demo(){
		System.out.println("Hello World");
		System.out.println("Hello World");
		System.out.println("Hello World");
		System.out.println("Hello World");
		System.out.println("Hello World");
		System.out.println("Hello World");
		System.out.println("Hello World");
		System.out.println("Hello World");
		System.out.println("Hello World");	
}		
{% endhighlight %}

Git is a version control system (VCS) for tracking changes in computer files and coordinating work on those files among multiple people. It is primarily used for source code management in software development, but it can be used to keep track of changes in any set of files. As a distributed revision control system it is aimed at speed, data integrity, and support for distributed, non-linear workflows.

```java
public class HelloWorld{
	
	public static void main(String[] args){

		System.out.println("Hello World");
	}
}
```


```python
import os
os.system('clear')
print("Python syntax highlighting")
a=5
```