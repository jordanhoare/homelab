## Quick start

### SSH key pairs
```zsh
ssh-keygen -t rsa -b 4096 # ignore if already exists
ssh-copy-id node@192.168.238.137
```

### Bootstrap
```zsh
task ansible:run namespace=bootstrap playbook=initialise
```

### Kubernetes
```zsh
task ansible:run namespace=kubernetes playbook=initialise
```
