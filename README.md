
### qdoractl 
```
To create, deploy and maintain your Qiniu ufop with ease. With this tool, you can:
    * Register/unregister an ufop;
    * Build and deploy an ufop;
    * List existing ufops;

Usage:
  ./qdoractl <command> [flags]

Login Commands:
  login       Login your account. You must login before running other commands.
  logout      Logout your account.

Essential Commands:
  deploy      List deploys or resize a running deploy.
  image       List docker images that have been uploaded.
  list        List ufops.
  push        Push a docker image to register the qiniu ufop repository.
  register    Register an ufop.
  release     List releases or create one release.

Other Commands:
  flavor      List flavors.
  log         Search ufop's logs.
  region      List regions.
  unregister  Unregister an ufop.

Flags:
  -v, --version   Show version of this tool.

Qiniu dora command-line tool, version: 0.2
```

### qdoractl login
```
you can login with Access Key and Secret Key:
    ./qdoractl -u <access key> -p <secret key>
If you don't provide necessary credentials, a command-line prompt will show for you to enter your access key and secret key.

Usage:
  ./qdoractl login [flags]

Flags:
  -u, --ak string   Your access key
  -p, --sk string   Your secret key.

Qiniu dora command-line tool, version: 0.2
```

### qdoractl logout
```
Logout your account.

Usage:
  ./qdoractl logout [flags]

Qiniu dora command-line tool, version: 0.2
```

### qdoractl list
```
List ufops.

Usage:
  ./qdoractl list [flags]

Qiniu dora command-line tool, version: 0.2
```

### qdoractl register
```
Register an ufop.

Usage:
  ./qdoractl register <ufop> [flags]

Flags:
  -d, --description string   Description of this ufop.
  -m, --mode string          ufop mode: private|public|protected. (default "private")

Qiniu dora command-line tool, version: 0.2
```

### qdoractl deploy
```
List deploys or resize a running deploy.

Usage:
  ./qdoractl deploy [flags]

Examples:
  ./qdoractl deploy <ufop> --all
  ./qdoractl deploy <ufop> --id <deployment_id>
  ./qdoractl deploy <ufop> <verstr> --region <region> --expect <num>


Flags:
  -a, --all             
  -e, --expect int       (default -1)
  -i, --id string       
  -r, --region string

Qiniu dora command-line tool, version: 0.2
```

### qdoractl image
```
List docker images that have been uploaded.

Usage:
  ./qdoractl image [flags]

Qiniu dora command-line tool, version: 0.2
```

### qdoractl push
```
Push a docker image to register the qiniu ufop repository.

Usage:
  ./qdoractl push <image> [flags]

Examples:
  ./qdoractl push ubuntu:14.04

Qiniu dora command-line tool, version: 0.2
```

### qdoractl release
```
List releases or create one release.

Usage:
  ./qdoractl release [flags]

Examples:
  ./qdoractl release <ufop>                list releases
  ./qdoractl release <ufop> <version>      view release
  ./qdoractl release --mkconfig .          create dora.yml
  ./qdoractl release --config .            build one release from dora.yml


Flags:
  -c, --config string   config path
  -d, --detail          show instances
  -i, --mkconfig        init dora.yml

Qiniu dora command-line tool, version: 0.2
```

### qdoractl flavor
```
List flavors.

Usage:
  ./qdoractl flavor [flags]

Qiniu dora command-line tool, version: 0.2
```

### qdoractl log
```
Search ufop's logs.

Usage:
  ./qdoractl log <ufop> <release> [flags]

Examples:
  ./qdoractl log <ufop> <version> --region xs --from=20161014032207
  ./qdoractl log <ufop> <version> --region xs --keyword ERROR
  ./qdoractl log <ufop> <version> --region xs --type stderr
  ./qdoractl log <ufop> <version> --region xs --type 0
  ./qdoractl log <ufop> <version> --region xs --show bunyan | bunyan


Flags:
      --from string          format: [<year><mont><day>hour><min>]. e.g. 201610170000
      --instance_id string   
  -k, --keyword string       e.g. ERROR
  -r, --region string        
      --show string          simple|detail (default "simple")
      --size uint             (default 100)
      --to string            format: [<year><mont><day>hour><min>]. e.g. 201610180000
      --type string          format: stdout|stderr|0

Qiniu dora command-line tool, version: 0.2
```

### qdoractl region
```
List regions.

Usage:
  ./qdoractl region [flags]

Qiniu dora command-line tool, version: 0.2
```

### qdoractl unregister
```
Unregister an ufop.

Usage:
  ./qdoractl unregister <ufop> [flags]

Qiniu dora command-line tool, version: 0.2
```
