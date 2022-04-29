# Jenkins Getting Started

## 2nd Edition Examples **[CURRENT EDITION]**

- Course repositories:
  - [2nd Edition INDEX / COURSE OVERVIEW repo](https://github.com/g0t4/course-jenkins-getting-started). 
    - I recommend cloning this repo and/or clicking links provided! 
    - Each edition of the course has its own set of repos.
    - **AVOID GOOGLING** or **GITHUBOOGLING** (is that a word? :smiley:) and instead follow these links 
  - [spring-petclinic - example notes](docs/spring-petclinic.md)
    - repo: https://github.com/g0t4/jgsu-spring-petclinic
  - https://github.com/jenkins-getting-started/jgsu-spc-jenkinsfile
    - `spring-petclinic` `pipeline` ported to a `Jenkinsfile` in `git`
  - https://github.com/jenkins-getting-started
    - This is a GitHub org I setup with several repos for the last demo of the course, to be scanned by Jenkins

## 2nd Edition docs/notes

See the [docs](docs) folder for various additional notes I've left for you. Quite a bit of this is stuff not in the course that I wanted to share with you as you get started. Some of it is fodder for learning beyond the course. 

## FAQ

- **I am having an issue with building code in the `jenkins2-...` repository, how do I fix that**
  - If you are watching the updated / 2nd Edition course (most likely). Use the updated repos too ([see above](https://github.com/g0t4/course-jenkins-getting-started/blob/master/README.md#2nd-edition-examples-current-edition)).
  - If you want a challenge or are watching the 1st edition, try to adapt the 1st edition repos to work with the 2nd edition examples! A great learning opportunity!

## 1st Edition 

- The 2nd edition is an update to my original (1st edition) [`Getting Started with Jenkins 2`](https://www.pluralsight.com/courses/jenkins-2-getting-started) course.
  - You can follow this link to access the original course which is longer and has some content that wasn't included in the update given that we wanted to shrink the duration for placement into a jenkins learning path. And of course include relevant new features where applicable.
  - FYI the vast majority of content in the 1st edition is still relevant in the latest versions of Jenkins!
- In the 2nd edition update, the `2` is dropped from the course title even though I'm still covering the latest version of Jenkins which is still `2`.
  - Accordingly I have dropped the `2` from repos and instead use `jgsu` in many cases as a prefix.
  - `jgsu` = `jenkins getting started update`
  - `jenkins2` was the prefix of many first edition repos.
- [1st Edition Overview/Resources Gist (https://git.io/vKSVZ)](https://git.io/vKSVZ)
  - I share these mostly to avoid confusion with associated repos and examples that are and will continue to remain accessibly here on GitHub.
- This `Jenkinsfile` highlights a complex pipeline with multiple nodes involved and parallelism, for a challenge try to get this up and running on your own. Or at least read through it and grok what you think is needed to get it working.


# My Notes

## Maven
Jenkins will need maven configured. In Global Tool Configuration we can easily ask it to automatically install the maven versions we want from Apache, while giving a name to each installation. For example, if we decide to use version 3.6.3, this will be printed on the logs on the first time:
```shell
Unpacking https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven/3.6.3/apache-maven-3.6.3-bin.zip to /var/jenkins_home/tools/hudson.tasks.Maven_MavenInstallation/maven_3.6.3 on Jenkins
```
Notice how Jenkins installs maven at <code>/var/jenkins_home/tools/hudson.tasks.Maven_MavenInstallation/maven_3.6.3</code> on the Jenkins instance.


## JDK

See [How to Configure a JDK in Jenkins](https://youtu.be/qx3XK82BZPk)

Jenkins itself is a Java application. Thus, it needs a JDK. My docker container running Jenkins comes with the following JDK Jenkins uses (see Manage Jenkins > System Information)
```shell
/opt/java/openjdk$ ls -larth
total 51M
drwxr-xr-x  3 root root 4.0K Apr 20  2021 lib
-r--r--r--  1 root root 152K Apr 20  2021 THIRD_PARTY_README
drwxr-xr-x 10 root root 4.0K Apr 20  2021 sample
drwxr-xr-x  4 root root 4.0K Apr 20  2021 man
-r--r--r--  1 root root  19K Apr 20  2021 LICENSE
drwxr-xr-x  4 root root 4.0K Apr 20  2021 jre
drwxr-xr-x  3 root root 4.0K Apr 20  2021 include
-r--r--r--  1 root root 1.5K Apr 20  2021 ASSEMBLY_EXCEPTION
-rw-r--r--  1 root root  51M Apr 20  2021 src.zip
-rw-r--r--  1 root root  333 Apr 20  2021 release
drwxr-xr-x  2 root root 4.0K Apr 20  2021 bin
drwxr-xr-x  3 root root 4.0K Jun 23  2021 ..
drwxr-xr-x  8 root root 4.0K Jun 23  2021 .
```
```shell
/opt/java/openjdk$ cat release 
JAVA_VERSION="1.8.0_292"
OS_NAME="Linux"
OS_VERSION="2.6"
OS_ARCH="amd64"
SOURCE=" .:OpenJDK: 008caa03f0:"
IMPLEMENTOR="AdoptOpenJDK"
BUILD_SOURCE="git:4ad15fd"
FULL_VERSION="1.8.0_292-b10"
SEMANTIC_VERSION="8.0.292+1"
BUILD_INFO="OS: Linux Version: 4.15.0-1113-azure"
JVM_VARIANT="Hotspot"
JVM_VERSION="25.292-b10"
IMAGE_TYPE="JDK"
```

Jenkins will use this JDK by default, if nothing is specified, for example in Freestyle project.

Other than the default JDK we can install ours under Global Tool Configuration. We'll have to set a name to each JDK installation so we can refer to it Jenkins files.

### JDK from a download URL
Another option is to specify the url of a JDK to download and install automatically. We do it under Add Installer > Extract *.zip/*.tar.gz. An example link for a JDK 8 could be<br>
<code>https://github.com/adoptium/temurin8-binaries/releases/download/jdk8u322-b06/OpenJDK8U-debugimage_x64_linux_hotspot_8u322b06.tar.gz</code>
See (https://github.com/adoptium/temurin8-binaries/releases/)[https://github.com/adoptium/temurin8-binaries/releases/] for JDK URLs from Adopt, for example. Look for assets with "x64_linux_hotspot" in its name.


### JDK from Oracle
Jenkins gives the option of automatically downloading and installing a JDK from Oracle, just that we'll need to provide our Oracle account credentials the first time. We'll see "Installing JDK requires Oracle account. Please enter your username/password". JDK installed this way will be places ad <code>/var/jenkins_home/tools/hudson.model.JDK</code>.

### Manual install of a JDK
To manually add a JDK, we can manually copy (extract the .tar.gz) a JDK into <code>/usr/lib/jvm</code>:
```shell
/usr/lib/jvm/jdk-11.0.12+1$ ls -larth
total 40K
drwxr-xr-x  4 root root 4.0K May  6  2021 man
drwxr-xr-x 73 root root 4.0K May  6  2021 legal
drwxr-xr-x  2 root root 4.0K May  6  2021 jmods
drwxr-xr-x  3 root root 4.0K May  6  2021 include
drwxr-xr-x  4 root root 4.0K May  6  2021 conf
-rw-r--r--  1 root root 1.5K May  6  2021 release
drwxr-xr-x  9 root root 4.0K May  6  2021 .
drwxr-xr-x  6 root root 4.0K May  6  2021 lib
drwxr-xr-x  2 root root 4.0K May  6  2021 bin
drwxr-xr-x  3 root root 4.0K Apr 29 10:22 ..
```
If we want to use it we can set it in Global Tool Configuration, specifying under <b>JAVA_HOME</b> the installation directory <code>/usr/lib/jvm/jdk-11.0.12+1</code>.

