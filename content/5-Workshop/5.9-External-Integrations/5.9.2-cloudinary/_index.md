---
title: "Verify product images with Cloudinary"
menuTitle: "Cloudinary Media"
weight: 2
chapter: false
pre: "<b> 5.9.2. </b> "
---

1. **Open Cloudinary Console → lab product environment → Media Library.** Create a lab folder, select Upload and upload one demo PNG/JPEG.

2. **Open the uploaded asset → Asset details.** Record public_id and secure_url. Store signed-upload credentials on the backend in Secrets Manager.

3. **In BTB open catalog administration → demo SKU.** Assign the image and save. Open the storefront and inspect both the product card and the detail page for that SKU.

4. **Open DevTools → Network → Img and refresh.** Select the image request and verify the Cloudinary hostname, Content-Type and transfer size.

**Verify.** The image loads after refresh. For a 404 inspect public_id and case. If the URL works but the card is blank check the API response and img src. Remove the SKU reference before deleting the lab asset.
