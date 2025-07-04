# set up

Tailscale has this neat new [k8s operator](https://tailscale.com/learn/managing-access-to-kubernetes-with-tailscale#using-tailscales-kubernetes-operator) so it was pretty simple to install it on the existing k3s cluster and set up a service to expose Jellyfin to the tailnet. 

![img/jellyfin-cats.png]

What can I say, it's pretty great.

I also tried [Cloudflare tunnels](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/) (using Ansible to provision it) so I could flex my vanity `milly.ashford.academy` domain. 

Both were pretty performant, Tailscale is fun to tinker with and it's probably easier to maintain over the long run when more services are up and getting them on to the tailnet is just a few lines in a manifest.