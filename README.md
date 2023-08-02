# Background

https://www.redhat.com/sysadmin/podman-inside-container

# Create Google VM in GCP Console
podman-rhel-8-8 (same as GCP Slaves)
RHEL-8
All APIs

# SSH into VM (as root)
gcloud compute ssh --zone "europe-west2-a" "podman-rhel-8-8-final" --project "k8s-play-unique"

useradd jenbld

dnf -yv install podman
podman info (as root)

remoteSocket:
    path: /run/podman/podman.sock

# SSH into VM (as jenbld)
gcloud compute ssh --zone "europe-west2-a" "jenbld@podman-rhel-8-8-final" --project "k8s-play-unique"

systemctl --user status podman.socket (expect disabled)
systemctl --user status podman.service (expect disabled)
systemctl --user enable podman.socket
systemctl --user enable podman.service
systemctl --user start podman.socket
loginctl enable-linger jenbld

podman info

remoteSocket:
  path: /run/user/1002/podman/podman.sock

-------------

[as jenbld]

vi Dockerfile

podman build --build-arg=CLOUD_SDK_VERSION=441.0.0 -t google-sdk-441-sans-docker .

podman run -it --rm \
  --security-opt label=disable \
  -v $XDG_RUNTIME_DIR/podman/podman.sock:/var/run/docker.sock \
    localhost/google-sdk-441-sans-docker:latest

[OPERATING CONTAINER-IN-CONTAINER]

Create your skaffold files manually from remote-skaffold, then:-

skaffold build -f skaffold.yaml

(Will create an image on the GCP VM NOT that will persist)