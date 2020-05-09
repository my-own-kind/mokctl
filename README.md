# My Own Kind

![](docs/images/mokctl-demo.gif)

## Summary

Build a conformant kubernetes cluster in containers, on Linux, using docker only, by hand, from scratch, for learning.

If you follow the instructions in this repository you will end up building your own version of [Kind](https://kind.sigs.k8s.io/). I'm pretty sure it won't be easy though - there's so much that can go wrong!

If you only want to use `mokctl` to build a kubernetes cluster then that's fine! See [below](#try-mokctl) for instructions for using on your OS. Please add to the open Issues if it does or doesn't work for you. Thanks!

To get the most out of the "labs", and I use that term loosely, you should have some theoretical understanding of kubernetes. Some good resources are:

* [Certified Kubernetes Administrator (CKA) with Practice Exam Tests](https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/)
* [Kubernetes in Action by Marko Luksa](https://www.goodreads.com/book/show/34013922-kubernetes-in-action)

Send a Pull Request to add to this list.

### Try mokctl

Install [Docker](https://docs.docker.com/get-docker/) if you don't have it already.

Take note of the [Status](#status) above and the [Releases](https://github.com/mclarkson/my-own-kind/releases) page.

#### For all Operating Systems

![](docs/images/install-linux.gif)

Note for Fedora users: Cgroups2 needs to be disabled, see [Common F31 bugs - Fedora Project Wiki](https://fedoraproject.org/wiki/Common_F31_bugs#Docker_package_no_longer_available_and_will_not_run_by_default_.28due_to_switch_to_cgroups_v2.29), which amounts to typing the following command then rebooting: `sudo grubby --update-kernel=ALL --args="systemd.unified_cgroup_hierarchy=0"`

Paste the following alias into your teminal:

```bash
alias mokctl='docker run --rm --privileged -ti -v /var/run/docker.sock:/var/run/docker.sock -v ~/.mok/:/root/.mok/ -e TERM=xterm-256color mclarkson/mokctl'
```

Then use mokctl:

```bash
mokctl build image

mokctl create cluster myk8s 1 0

export KUBECONFIG=~/.mok/admin.conf

kubectl get pods -A

kubectl run -ti --image busybox busybox sh
```

See: [Mokctl on Docker Hub](https://hub.docker.com/r/mclarkson/mokctl).

#### For Linux only

To build and install from source:

```bash
git clone https://github.com/mclarkson/my-own-kind.git
cd my-own-kind
```

Then EITHER build your own `mokctl` docker image and add a bash/zsh alias to it:

```bash
make mokctl-docker

alias mokctl='docker run --rm --privileged -ti -v /var/run/docker.sock:/var/run/docker.sock -v ~/.mok/:/root/.mok/ -e TERM=xterm-256color local/mokctl'
```

OR, install into `/usr/local/bin`:

```bash
make test
sudo make install
```

Then use `mokctl`:

```bash
mokctl build image
mokctl create cluster myk8s 1 0
```

Removal

```bash
# To remove mokctl
sudo make uninstall

# OR, to remove mokctl and the `~/.mok/` configuration directory
sudo make purge
```

Also remove the docker images if required:

* local/mokctl

* local/mok-centos-7-v1.18.2

* docker

* centos

## Status

**Mokctl Utility**

| OS        | Termnal          | Status |
| --------- | ---------------- | ------ |
| Fedora 31 | Gnome Terminal   | Works  |
| Mac OS    | Default terminal | ?      |
| Windows   | Cygwin           | ?      |

**Documentation/Labs**

In progress.

## Let's get started

1. [Single Node](docs/build.md)
   
   Jump straight into setting up a docker container for a single node kubernetes cluster.

2. [Package](docs/package.md)
   
   We package up what we have learned ready for the next task.

3. [Testing and Fixing](/docs/testfix.md)
   
   This thing has bugs. Let's fix it.

4. [Kubernetes the Hard Way](/docs/k8shardway.md)
   
   The classic remade for MOK. A 7 node cluster without blowing up your laptop!

5. [Multi Node]()
   
   Build a bigger cluster using our package.

6. [Upgrade](/docs/upgrade.md)
   
   We can install any version of kubernetes. Let's try installing an older cluster and upgrading it.

7. [Add-ons](/docs/addons.md)
   
   Trying popluar kubernetes add-ons.

## Contributing

Please check the 'Help Wanted' issues if you fancy helping.

All types of contributions are welcome, from bug reports, success stories, feature requests, fixing typppos, to coding. Also check the [CONTRIBUTING.md](/CONTRIBUTING.md) document.

## License

Apache 2 - You can do what you like with the software, as long as you include the required notices. This permissive license contains a patent license 
from the contributors of the code. See: [tl;drLegal](https://tldrlegal.com/license/apache-license-2.0-%28apache-2.0%29) for a summary. Full text is in the LICENSE file.

## Why?

I started this project to improve my understanding of [kubernetes](https://kubernetes.io/), and to help me pass the [Certified Kubernetes Administrator](https://www.cncf.io/certification/cka/) exam. It's an amalgamation of information gleaned from:

* The kubernetes local container installer, [Kind](https://kind.sigs.k8s.io/).
* The official Kubernetes documentation at [kubernetes.io/docs/.../install-kubeadm/](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/).
* Mumshad Mannambeth's [kubernetes-the-hard-way](https://github.com/mmumshad/kubernetes-the-hard-way) - using VirtualBox.
  I also took his course on Udemy, [Certified Kubernetes Administrator (CKA) with Practice Exam Tests](https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/), which I highly recommended if you're new to kubernetes.
* Kelsey Hightower's [kubernetes-the-hard-way](https://github.com/kelseyhightower/kubernetes-the-hard-way) - using GCP. Check out the number of stars on this project!
* Other official sources (all shown).

I wanted something quite particular to my own needs/wants:

* To set up a full kubernetes cluster inside containers.
  
  So it works on my under-powered laptop.

* To use [cri-o](https://cri-o.io/) instead of docker/containerd.
  
  It looks like CRI-O is the future.

* To use [kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm/).
  
  To get a verifiably compliant kubernetes cluster running.
