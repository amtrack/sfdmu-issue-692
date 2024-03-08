# [SFDMU] Minimum Reproduction Example: Importing from a CSV file fails when the target org has an additional field (e.g. of type Checkbox)

Bug report: https://github.com/forcedotcom/SFDX-Data-Move-Utility/issues/692

## Instructions for reproduction in a Scratch Org

### Optional: Exporting to CSV files

> The CSV files are already stored in this Git repository in the `data` folder.

```console
sf org create scratch -f config/project-scratch-def.json -a sfdmu-export-to-csv -d
sf data record create -s Account -v "Name='ACME'"
# export Accounts
sf sfdmu run -p data -s sfdmu-export-to-csv -u csvfile --filelog 0 -n
```

### Importing from CSV files

```console
sf org create scratch -f config/project-scratch-def.json -a sfdmu-csv-import -d
# try importing: it works
sf sfdmu run -p data -s csvfile -u sfdmu-csv-import --filelog 0 -n
# clean up
sf data record delete -s Account -w "Name='ACME'"
# now deploy a new Checkbox field on Account
sf project deploy start
sf org assign permset -n Account_Test
# try importing again: it fails
sf sfdmu run -p data -s csvfile -u sfdmu-csv-import --filelog 0 -n
```
