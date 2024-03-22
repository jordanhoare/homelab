# Network Segmentation and Zero-Trust Access
Future network enhancements will focus on implementing network segmentation to isolate the Kubernetes cluster. This isolation helps mitigate risks and is a step towards a zero-trust network model. Implementing VLANs or similar technologies will provide fine-grained control over network traffic, aligning with zero-trust principles by verifying and minimizing access scopes.

## Cloudflare Tunnel Integration
An additional layer of security and accessibility will be considered through [Cloudflare Tunnel](https://www.cloudflare.com/en-au/products/tunnel/). This will create secure and private connections between the cluster and the Cloudflare edge, enabling safe exposure of services without exposing the cluster directly to the public internet. This complements the zero-trust approach and provides secure access to services from anywhere, without VPN overhead.

