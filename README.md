# Docker Image-Signing
Signing docker images is an important step in software supply chain security. Incorporating image signing controls with K8s admission controllers like Kyverno or OPA gatekeeper ensures that only verified images are deployed to clusters. 
This significantly reduces the likelihood of malicious workloads being deployed into otherwise secure environments. 
## Tooling
[Cosign](https://github.com/sigstore/cosign) is the main tool of focus here for signing images. Cosign works by using a private key to write a signature to the image repository. This signature can then
be verified by using the public key.

To get started with cosign, install it using the commands as shown below:

### Linux binary Install steps
`curl -O -L "https://github.com/sigstore/cosign/releases/latest/download/cosign-linux-amd64"`

`sudo mv cosign-linux-amd64 /usr/local/bin/cosign`

`sudo chmod +x /usr/local/bin/cosign`

### generate cosign key pair
`cosign generate-key-pair`

### how to sign images
`cosign sign --key cosign.key registry/image@digest --registry-token=token -y`

### verify image signatures
`cosign verify --key cosign.pub registry/image:tag --registry-token=token`

### Further comments
It is always good practice to have the generated private key stored in a secrets manager like Vault or AWS secrets manager. The ideal workflow for signing images is to include the signing commands after CI commands that push the images to the given repository.
Included in this repository is a [sample-signing.yaml](https://github.com/zazathomas/docker-image-signing/blob/main/.github/workflows/sample-signing.yaml) file which is a GitHub actions workflow for signing images after pushing to dockerhub. Reuse this file by modifying the image, public & private keys as required.
