# ansible-role-duply

## Install
```
ansible-galaxy install tseho.duply
```

## Usage
See [defaults](defaults/main.yml) for available options.
See [duplicity manpage](http://duplicity.nongnu.org/duplicity.1.html) for more documentation.

## Profiles
```
duply_profiles:
  - name: "<name>"
    source: "<directory>"
    target: "<target>"
    params:
      - "DUPL_PARAMS=\"$DUPL_PARAMS --copy-links\""
    exclude:
      - "<directory>/<path>"
    cronjobs:
      - schedule: 0 3 * * *
  - name: "<name>"
    source: "mysql://<database_name>"
    target: "<target>"
    params: []
    cronjobs:
      - schedule: 0 3 * * *
        action: pre+full+post # see Cronjob action
```

### Source

Only directories and `mysql://` are currently supported.

### Cronjob action

For each cronjob, default action is `backup`.

When you want to do a full backup of the database every time, do `pre+full+post`.
`full` does not include `pre` and `post` and these are the hooks for handling
the database dump.

### Target

#### AWS S3 example

```
duply_profiles:
  - name: "<name>"
    source: "<directory>"
    target: "s3+http://<bucket>/<path>"
    params:
      - "DUPL_PARAMS=\"$DUPL_PARAMS --copy-links\""
      - "DUPL_PARAMS=\"$DUPL_PARAMS --s3-use-new-style --s3-european-buckets\""
      - "export AWS_ACCESS_KEY_ID='<aws_key>'"
      - "export AWS_SECRET_ACCESS_KEY='<aws_secret>'"
```

Of course, It's recommended to retrieve the aws credentials from your **vault**.
