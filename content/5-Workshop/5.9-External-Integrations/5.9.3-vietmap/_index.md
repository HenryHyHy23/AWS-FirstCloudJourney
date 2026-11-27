---
title: "Verify address search and coordinates with Vietmap"
menuTitle: "Vietmap Maps & Location"
weight: 3
chapter: false
pre: "<b> 5.9.3. </b> "
---

1. **Choose two public places as the demo warehouse and destination.** In the lab Vietmap project check access to place search/routing. Keep the server key in the backend secret and configure suitable domain restrictions on any frontend SDK key.

2. **Open the BTB delivery address form.** Enter the first place, select a suggestion and compare its ward/province, coordinates and marker.

3. **Select and save the second place.** If the UI supports routing record distance, unit and ETA source. Try an unmatched input and edit the address again.

**Verify.** The marker matches the selected location. For a misplaced marker check longitude/latitude order against the API. For 403 check access/key restrictions; for 429 check quota.