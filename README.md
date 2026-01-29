# Apple Photos Pain

Apple Photos on Mac said: "Syncing 4 Items to iCloud" and that NEVER went away. 

**Apple Photos Version**: Version 10.0 (770.0.172)

**MacOS Version**: 15.7.3

To find the culprit photos this is what I did:

## Use the magic query

```bash
sqlite3 ~/Pictures/Photos\ Library.photoslibrary/database/Photos.sqlite "SELECT ZORIGINALFILENAME, ZCREATIONDATE, ZIMPORTDATE, ZCLOUDMASTERGUID FROM ZCLOUDMASTER WHERE ZCLOUDLOCALSTATE = 1;"
```

This should give you all the files that are giving you iCloud issues. The number of rows in this table should match up with the number of items that are failing to sync to iCloud.

## Open the troublesome photo(s) with magic command one-liner

```bash
sqlite3 ~/Pictures/Photos\ Library.photoslibrary/database/Photos.sqlite "SELECT ZORIGINALFILENAME, ZCLOUDMASTERGUID FROM ZCLOUDMASTER WHERE ZCLOUDLOCALSTATE = 1;" | while IFS="|" read -r fname guid; do [ -n "$guid" ] && open ~/Pictures/Photos\ Library.photoslibrary/resources/cpl/cloudsync.noindex/storage/filecache/"${guid:0:3}"/cpl"${guid}".*; done
```

This bad boy should open those troublesome photos so you can know exactly which ones they are, track them down, and DESTROY them!

---

Will this work for you? I have ZERO idea. I was just tired of this issue on my end and with loyal friend Mr. GPT I was able to quickly look around all the tables and schemas and using their helpful (not really) column names eventually get down to the culprits. Hope this helps someone out.
