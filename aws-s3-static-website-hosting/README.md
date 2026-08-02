# Host a Website on Amazon S3

**Author:** Richard Lutendo Mudau
**Full guided walkthrough:** [NextWork Project](http://nextwork.ai/projects/aws-host-a-website-on-s3)

**Skills:** Amazon S3 · Static Website Hosting · Bucket Policies · ACLs · Cloud Security · Troubleshooting

---

## Project Overview

In this project I demonstrated how to use Amazon S3 to host a static website. I did this project to learn how AWS cloud services can be used to store objects in the cloud — and even host websites.

**Services used:** Amazon S3
**Key concepts learnt:** bucket policies, uploading static website files, index.html, bucket endpoint URLs, and ACLs (how they control access to my bucket).

**Time, challenges and wins:** ~1.5 hours including quiz and secret-mission time. The most challenging part was resolving the **403 Forbidden error**. The most rewarding part was seeing my webpage load live, public to the world.

---

## 1. Setting Up the S3 Bucket

I opened S3 and created a bucket to store the website files. Creating the bucket took less than 5 minutes once I had learnt the key concepts (ACLs and Block Public Access).

- **Region:** Stockholm (closest AWS region to me)
- **Bucket names are globally unique** — no one else in the world can use the same name.

![S3 bucket creation](http://nextwork.ai/cheerful_lavender_brave_skunk/uploads/aws-host-a-website-on-s3_ba6d42ad)

## 2. Uploading the Website Files

I uploaded the website into my S3 bucket — no files means no website.

**Files uploaded:**
- `index.html` — defines the structure and content of the website
- A folder of images/assets — the content the page displays

Both are necessary: the HTML says "insert image here", but without the actual image asset uploaded separately, the page has nothing to show.

![Files uploaded to bucket](http://nextwork.ai/cheerful_lavender_brave_skunk/uploads/aws-host-a-website-on-s3_a265af88)

## 3. Enabling Static Website Hosting

Without this step, my files stay as files — not a website. Website hosting means putting the files on infrastructure that turns them into a page people can visit.

To enable it, I went to the **Properties** tab of my bucket, enabled static website hosting, and set `index.html` as the index document.

**About ACLs:** an ACL configures permission settings for objects inside a bucket. I enabled ACLs so I could control access to my website files. AWS showed a pop-up recommending ACLs stay disabled — I kept them enabled deliberately to learn how ACLs work and compare them with bucket policies later.

![Static website hosting enabled](http://nextwork.ai/cheerful_lavender_brave_skunk/uploads/aws-host-a-website-on-s3_c22c54c0)

## 4. Testing the Bucket Endpoint

Once static hosting is enabled, S3 produces a **bucket endpoint URL** — the address that takes anyone on the internet to the hosted website.

**What I saw first: a 403 Forbidden error.** The reason: even after switching off "Block all public access", the objects themselves are still private — their access must be managed separately. The files need to be public too for the world to see the website.

![403 error at endpoint](http://nextwork.ai/cheerful_lavender_brave_skunk/uploads/aws-host-a-website-on-s3_22ce4daf)

## 5. Resolving the 403 Error ✅

I updated the access settings of the files inside the bucket — using ACLs I made the website files public. After that, the bucket endpoint loaded the full webpage. **Site officially live.**

![Website live](http://nextwork.ai/cheerful_lavender_brave_skunk/uploads/aws-host-a-website-on-s3_5d4474f9)

## 6. Extension: Protecting Files with a Bucket Policy

For the project extension, I used a **bucket policy** to control access to my bucket's files — specifically to stop anyone from deleting objects.

Bucket policies are rules that determine who is allowed (or not allowed) to do something. Compared to ACLs (which control individual objects), bucket policies give greater control over which actions are allowed or denied.

**My bucket policy denies *everyone* from deleting `index.html`.** I tested it by attempting to delete the file and received a permission-denied error — proof the policy worked and the object is protected.

![Bucket policy in place](http://nextwork.ai/cheerful_lavender_brave_skunk/uploads/aws-host-a-website-on-s3_sm2sm2sm)

---

## Key Takeaways

- S3 can host static websites with zero servers to manage
- "Block Public Access" and object-level permissions are **separate layers** — both must be handled to go public safely
- ACLs control individual objects; bucket policies control actions across the bucket — knowing when to use each is a real cloud security skill
- The 403 Forbidden error is almost always a permissions problem, not a hosting problem
