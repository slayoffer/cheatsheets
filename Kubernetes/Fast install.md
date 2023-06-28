#### **Installing Kubernetes and a text editor**

Docker Desktop is the best way to get a local Kubernetes single-node install on macOS, Windows, and many Linux desktop distributions. If you don't already have "DD" installed, Docker already has some [great guides on how to do it](https://docs.docker.com/get-docker/), but if you already know which OS you want to install on, below are quick steps for downloading it. **Feel free to skip any of this Section if you have Kubernetes and Visual Studio Code already installed.**

**I always recommend** [**Visual Studio Code (vscode) as your editor**](https://code.visualstudio.com/) for all things Docker, Kubernetes, and DevOps. It's free for all OSs and has plugins to help you with many tools in this course.

#### Installing on Windows 10 and Windows 11 (any Edition)

[https://docs.docker.com/desktop/windows/install/](https://docs.docker.com/desktop/windows/install/)

This is the best experience on Windows, which [uses WSL2](https://docs.microsoft.com/en-us/windows/wsl/install). If you're new to WSL2, it's the best way to run Linux on Windows, and Docker Desktop will walk you through enabling it. Docker can technically still work on Hyper-V with "Windows Containers," but most of this course focuses on Linux Containers, which WSL2 is great at.

After installing [Docker Desktop for Windows](https://docs.docker.com/desktop/windows/install/). Make sure you enable Kubernetes in the settings.

I also recommend installing a Windows Store [Ubuntu for WSL2](https://www.microsoft.com/en-us/p/ubuntu/9pdxgncfsczv?rtc=1&activetab=pivot:overviewtab) distro and using its shell in [Windows Terminal](https://www.microsoft.com/en-us/p/windows-terminal/9n0dx20hk701#activetab=pivot:overviewtab) is the best CLI experience! Make sure to check your Docker Desktop settings to give it enough resources and enable all your WSL2 distros for Docker Desktop.

#### Installing on Windows 7 or Windows 8

Unfortunately, Microsoft's OS features for Docker Desktop don't work in these older versions. You'll need to use Hyper-V or install VirtualBox and manually setup a Linux VM. It's more involved than the other options, so I often recommend the easier option: using a DigitalOcean cloud server with my coupon code [https://m.do.co/c/b813dfcad8d4](https://m.do.co/c/b813dfcad8d4) gets you $100 in credits over 60 days.

#### Installing on Mac

[https://docs.docker.com/desktop/mac/install/](https://docs.docker.com/desktop/mac/install/)

You'll want to install [Docker Desktop for Mac](https://docs.docker.com/desktop/mac/install/), which is great.

#### Installing on Linux Desktop

**2022 update**: Docker Desktop for Linux (with Kubernetes) is here!

[https://docs.docker.com/desktop/linux/install/](https://docs.docker.com/desktop/linux/install/)

#### Installing Kubernetes on Linux Server (local or remote)

[https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)

**Do *not* use your built-in default packages**  `**apt/yum install docker.io**`  because those packages are old and not the Official Docker-Built packages. 

I prefer to use [Docker's automated script](https://get.docker.com/) to add their repository and install all dependencies: `curl -sSL https://get.docker.com/ | sh`  but you can also install in a more manual method by following specific instructions that Docker provides [for your Linux distribution](https://docs.docker.com/engine/install/).

#### minikube

[https://minikube.sigs.k8s.io/docs/](https://minikube.sigs.k8s.io/docs/)

- Minikube is great, and manages a local VM it creates just for Kubernetes. Once installed, you can create the VM with `minikube start`
    
- Unlike Docker Desktop, which lets you use localhost for your Kubernetes services, minikube runs in VirtualBox (by default) and has its own IP. Find that with `minikube ip`
    
- Remember top stop minikube when you're not using it to save resources `minikube stop`
    
- Check the status of what is running in minikube with `minikube status`
    

#### MicroK8s

[https://microk8s.io/](https://microk8s.io/)

- MicroK8s is great for dedicated VMs or when you've created a VM locally with [Multipass](https://multipass.run/)
    
- MicroK8s installs the latest Kubernetes release with  `sudo snap install microk8s --classic`
    
- Before using it, you'll need to enable the CoreDNS pod so it'll resolve service DNS names later: `microk8s enable dns`
    
- Check the status of what is running in MicroK8s with `microk8s status`
    

  

#### Use Play With Kubernetes

[https://labs.play-with-k8s.com/](https://labs.play-with-k8s.com/)

Maybe you don't have local admin, or maybe your machine doesn't have enough resources. Well, the best free option here is to use Play with Kubernetes, which will run one or more Kubernetes instances inside your browser with `kubeadm`, and give you a terminal to use it with. You can create multiple machines on it and even use the URL to share the session with others in a sort of collaborative experience.  I highly recommend you check it out.  Most of the lectures in this course can be used with "PWK8s," but the **big limitation is it'll reset after 4 hours, at which time it'll delete your servers.**

#### Use DigitalOcean as a remote Linux Server

If you signup for a new account with DigitalOcean, my favorite "simpler" cloud, you can use a smaller server for months with my signup credit. **Install MicroK8s in the Cloud, via the above Linux links, and do the course without needing Docker locally.** Thousands of students have done this: 

[https://m.do.co/c/b813dfcad8d4](https://m.do.co/c/b813dfcad8d4) gets you $100 in credits over 60 days