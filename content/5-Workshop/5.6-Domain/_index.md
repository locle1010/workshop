---
title: "Custom Domain (Optional)"
date: 2026-07-04
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

#### Overview

> **This step is optional.** Skip it if you don't have a custom domain.

In this step, you will attach a custom domain (e.g., `pet.yourdomain.com`) to the CloudFront Distribution and issue a free SSL certificate via **AWS Certificate Manager (ACM)**.

---

### Requirements

+ You already have a domain name (purchased from Route 53 or another provider)
+ The domain's Name Servers already point to **AWS Route 53**

Check the managed Hosted Zone:
```bash
aws route53 list-hosted-zones-by-name \
  --dns-name "yourdomain.com" \
  --query "HostedZones[0].{Name:Name,Id:Id}" \
  --output table
```

![Check Hosted Zone](/images/5-Workshop/5.6-Domain/5.6.1.png)

---

### Create SSL Certificate (ACM)

> **Required:** CloudFront only accepts Certificates created in Region **`us-east-1` (N. Virginia)**.

```bash
aws acm request-certificate \
  --domain-name "yourdomain.com" \
  --subject-alternative-names "*.yourdomain.com" \
  --validation-method DNS \
  --region us-east-1 \
  --query "CertificateArn" \
  --output text
```

 **Save the `CertificateArn`**

![Create SSL Certificate](/images/5-Workshop/5.6-Domain/5.6.2.png)

Get DNS validation record:
```bash
aws acm describe-certificate \
  --certificate-arn <CertificateArn> \
  --region us-east-1 \
  --query "Certificate.DomainValidationOptions[0].ResourceRecord"
```

Output format:
```json
{
    "Name": "_xxxx.yourdomain.com.",
    "Type": "CNAME",
    "Value": "_yyyy.acm-validations.aws."
}
```

![Get DNS validation record](/images/5-Workshop/5.6-Domain/5.6.3.png)

---

### Validate Certificate via DNS

#### Using Route 53

**Console:** ACM → Select certificate → Click **Create records in Route 53** → Create records

![Select certificate in ACM](/images/5-Workshop/5.6-Domain/5.6.4.png)

![Create DNS records in Route 53](/images/5-Workshop/5.6-Domain/5.6.5.png)

![Confirm record creation](/images/5-Workshop/5.6-Domain/5.6.6.png)

![Records created successfully](/images/5-Workshop/5.6-Domain/5.6.7.png)

Wait 2-5 minutes for the status to change to **Issued**

![Certificate validated (Issued)](/images/5-Workshop/5.6-Domain/5.6.8.png)

---

### Attach Certificate to CloudFront

**Console:** CloudFront → Distribution → **Add domains**

![Open Distribution to add domain](/images/5-Workshop/5.6-Domain/5.6.9.png)

![Add domains screen](/images/5-Workshop/5.6-Domain/5.6.10.png)

+ **Configure domains:** Add `yourdomain.com` and `www.yourdomain.com`

![Configure domain names](/images/5-Workshop/5.6-Domain/5.6.11.png)

+ **Get TLS certificate:** Select the ACM certificate just created (only shows certificates in us-east-1)

![Select TLS certificate](/images/5-Workshop/5.6-Domain/5.6.12.png)

+ Click **Add domains**

![Confirm adding domain](/images/5-Workshop/5.6-Domain/5.6.13.png)

+ Wait for CloudFront to finish updating, then click **Route domains to CloudFront**

![Route domains to CloudFront](/images/5-Workshop/5.6-Domain/5.6.14.png)

+ Select **Set up routing automatically**

![Configure automatic routing](/images/5-Workshop/5.6-Domain/5.6.15.png)

---

### Final Backend CORS Update

After the domain is active, update `backend/.env.production`:
```env
FRONTEND_URL=https://yourdomain.com
```

Re-deploy:
```bash
cd backend
npx serverless deploy --stage prod
```

---

### Verify Results

After ~5-10 minutes for DNS propagation, visit `https://yourdomain.com` to verify.

![Website running on custom domain](/images/5-Workshop/5.6-Domain/5.6.16.png)
