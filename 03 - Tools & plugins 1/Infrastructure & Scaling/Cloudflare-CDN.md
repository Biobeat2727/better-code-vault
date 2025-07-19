# Cloudflare / CDN

**CDNs (Content Delivery Networks)** cache and deliver static content from servers around the world. **Cloudflare** is a popular CDN that also offers DNS, caching, SSL, security, and serverless compute.

---

## 🚀 Why Use a CDN?

- Reduce latency for global users
- Offload static content from origin server
- Speed up load times (JS, CSS, images, fonts)
- Handle spikes in traffic gracefully

---

## 🛠 Cloudflare Features

- DNS and reverse proxy
- Free SSL and DDoS protection
- Edge Functions (serverless logic)
- Page Rules and Cache-Control

---

## 💡 Best Practices

- Cache static assets aggressively
- Use versioned URLs (`/v1/main.js`)
- Set cache headers with long TTLs
- Use `stale-while-revalidate` for freshness

---

## 📚 Resources

- https://developers.cloudflare.com
