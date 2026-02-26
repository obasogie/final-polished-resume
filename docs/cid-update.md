# Updating the Site When the IPFS CID Changes

## TL;DR one-liner (commit message)
CID rotated → updated ipfs-cif.txt and eth.html to new CID: <NEW_CID>

## Files involved
- ipfs-cid.txt — contains ONLY the latest CID (no spaces/newlines if possible)
- eth.html — tiny redirector to the latest CID

## Standard procedure (manual)
1) Upload the new site build to IPFS. Copy the new root CID: <NEW_CID>.
2) Open ipfs-cid.txt and replace its content with <NEW_CID> (exact CID only).
3) Ensure eth.html redirects to whatever is in ipfs-cid.txt (template below).
4) Commit: `CID rotated → updated ipfs-cid.txt and eth.html to new CID: <NEW_CID>`
5) Push to GitHub 

## ipfs.html (redirector template)
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Redirecting to latest IPFS</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <script>
    // Fetch latest CID from ipfs-cid.txt and redirect
    fetch('ipfs-cid.txt', {cache: 'no-store'})
      .then(r => r.text())
      .then(cid => {
        cid = cid.trim();
        if (cid) location.replace('https://ipfs.io/ipfs/' + cid + '/');
        else document.body.textContent = 'Missing CID in ipfs-cid.txt';
      })
      .catch(() => document.body.textContent = 'Unable to read ipfs-cid.txt');
  </script>
  <noscript>
    This page redirects to the latest IPFS CID listed in ipfs-latest.txt.
    Please enable JavaScript or open https://ipfs.io/ipfs/<CID_HERE>/
  </noscript>
</head>
<body style="font:16px/1.4 system-ui, Arial">
  Redirecting to latest IPFS content…
</body>
</html>
