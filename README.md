# AWS Student Builders Group — Week 2 Lab
## Networking & IAM Fundamentals

**Name:** Edom  
**ID:** ETS0466/17  

## Task 1 — Identify Network Information

**Operating System:** Arch Linux

I used `ip addr` to inspect my network interfaces and identify my local IPv4 address.

- **Local IPv4:** [Enter from screenshot]
- **Default gateway:** [Enter if verified]
- **Address type:** [Private or public]

**Evidence:** `evidence/network-info.png`

## Task 2 — Practice Subnetting

**Starting network:** `10.0.0.0/24`

The network was divided into four equal-sized subnets by borrowing two host bits:

- **New prefix:** `/26`
- **Total addresses per subnet:** \(2^{32-26}=64\)
- **Conventional usable hosts:** \(64-2=62\)
- **AWS usable IPv4 addresses:** \(64-5=59\)

| Subnet | Network address | Full address range | Usable host range | Broadcast address |
|---|---|---|---|---|
| 1 | `10.0.0.0/26` | `.0–.63` | `.1–.62` | `.63` |
| 2 | `10.0.0.64/26` | `.64–.127` | `.65–.126` | `.127` |
| 3 | `10.0.0.128/26` | `.128–.191` | `.129–.190` | `.191` |
| 4 | `10.0.0.192/26` | `.192–.255` | `.193–.254` | `.255` |

Detailed subnet calculations are shown in my scanned handwritten work.

**Evidence:** `evidence/subnet-calculations.jpg`

## Task 3 — Test DNS Resolution

**Command used:**

```bash
nslookup example.com
```

- **Domain tested:** `example.com`
- **DNS resolution:** [Success or failure]
- **Returned IP address(es):** [Enter from screenshot]

DNS translates domain names into IP addresses, allowing computers to locate servers.

**Evidence:** `evidence/dns-lookup.png`

## Task 4 — Inspect an HTTPS Website

**Website:** [https://example.com](https://example.com)

**Command used:**

```bash
curl -I https://example.com
```

- **Website accessible:** [Record observed result]
- **HTTP status code:** [Enter actual status code]

HTTPS uses TLS to encrypt communication, protect data integrity, and authenticate the server. DNS resolution alone does not guarantee a successful connection because network connectivity, TLS negotiation, and server responses can still fail.

**Evidence:** `evidence/https-response.png`

## Task 5 — Interpret HTTP Status Codes

| Status code | Meaning | Example situation |
|---|---|---|
| `200` | OK — request succeeded | A webpage loads successfully. |
| `301` | Moved Permanently | A page redirects to a new URL. |
| `403` | Forbidden | Access to a restricted resource is denied. |
| `404` | Not Found | The requested page does not exist. |
| `500` | Internal Server Error | The server encounters an unexpected problem. |

**Difference between common failures:**

- **DNS failure:** The domain cannot be resolved to an IP address.
- **Connection timeout:** The destination does not respond within the expected time.
- **TLS certificate error:** The secure connection fails certificate validation or negotiation.
- **HTTP error:** The server returns an HTTP response indicating that the request failed or cannot be fulfilled.

## Task 6 — Write a Least-Privilege IAM Policy

I created `s3-read-policy.json` with permission to read objects from `example-bucket`.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::example-bucket/*"
    }
  ]
}
```

**Policy questions:**

1. **Permitted action:** The `Action` field specifies `s3:GetObject`.
2. **Resources:** The `Resource` field identifies objects in `example-bucket`.
3. **Does it allow listing all objects?** No. Listing requires the separate `s3:ListBucket` permission.
4. **Why is least privilege safer?** It limits access to only the actions and resources required, reducing the risk of unauthorized changes or data exposure.
5. **Why should AWS keys never be committed to GitHub?** Exposed credentials may allow unauthorized access, data theft, resource modification, or unexpected charges.

This policy was created for educational purposes and was not attached to a live AWS identity.

## Task 7 — Troubleshooting Practice

| Scenario | First layer to investigate | Reason |
|---|---|---|
| A domain does not resolve. | DNS | The hostname cannot be translated into an IP address. |
| A domain resolves, but the browser times out. | Network connectivity | Routing, firewalls, ports, or server availability may be the cause. |
| The browser shows a TLS certificate warning. | TLS | The certificate may be expired, untrusted, or invalid for the hostname. |
| A website returns `404`. | HTTP/application | The requested resource may not exist at that URL. |
| An EC2 application receives `AccessDenied` from S3. | IAM/S3 authorization | The application's role or applicable S3 permissions may not allow the operation. |

## Repository Structure

```text
week-2-networking-iam-lab/
├── README.md
├── s3-read-policy.json
└── evidence/
    ├── subnet.jpg
    ├── week2a.jpg
    ├── week2b.jpg
    └── week2c.jpg
```