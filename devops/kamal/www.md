# Handling www to non-www redirects with Kamal 2

## Problem

When deploying applications with Kamal 2, you might need to redirect www to non-www URLs (or vice versa). Unlike its predecessor which used Traefik, Kamal 2's kamal-proxy doesn't currently support redirects directly.

## Solutions

Based on the documentation and discussion, there are currently a few ways to handle www to non-www redirects with Kamal 2, since kamal-proxy doesn't directly support redirects yet:

### Use Cloudflare Page Rules (Recommended if using Cloudflare)

- Create a page rule in Cloudflare dashboard
- Set "If URL matches": www.yourdomain.com/*
- Choose "Forwarding URL" with 301 permanent redirect
- Set destination URL: https://yourdomain.com/$1

### Use an external load balancer

- Set up Nginx or Caddy as a load balancer in front of kamal-proxy
- Configure redirect rules in the load balancer config

### Handle in application code

- Implement the redirect logic in your application (e.g. in Rails)
- This adds some overhead since requests still hit your application

## Summary

The cleanest current solution is using Cloudflare page rules if you're already using Cloudflare. This handles the redirect at the edge before requests hit your servers.
There's an open feature request ([#42](https://github.com/basecamp/kamal-proxy/discussions/42)) to add redirect support directly in kamal-proxy, but it's not implemented yet. Until then, the recommended approaches are:

- Use Cloudflare page rules (preferred if using Cloudflare)
- Use an external load balancer with redirect rules
- Handle redirects in application code (less optimal)

The documentation suggests kamal-proxy may add this capability in the future, but for now you'll need to use one of these workaround approaches.
