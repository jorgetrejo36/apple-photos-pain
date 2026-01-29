# Apple Photos Pain

Apple Photos on Mac said: "Syncing 4 Items to iCloud" and that NEVER went away. 

**Apple Photos Version**: Version 10.0 (770.0.172)

**MacOS Version**: 15.7.3

To find the culprit photos this is what I did:

## Open sqlite3 for the photos

This worked on my Mac and is probably similar on your's (from your home directory):

```bash
sqlite3 Pictures/Photos\ Library.photoslibrary/database/Photos.sqlite
```

## Use the magic query

```bash
ELECT ZORIGINALFILENAME, ZCREATIONDATE, ZIMPORTDATE, ZCLOUDMASTERGUID FROM ZCLOUDMASTER WHERE ZCLOUDLOCALSTATE = 1;
```

This should give you all the files that are giving iCloud issues.

## Open the troublesome photo(s)

```bash
open "photos://asset/<ZCLOUDMASTERGUID>"
```

---

Will this work for you? I have ZERO idea. I was just tired of this issue on my end and with loyal friend Mr. GPT I was able to quickly look around all the tables and schemas and using their helpful (not really) column names eventually get down to the culprits. Hope this helps someone out.
