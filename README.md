# terraform-xp

### Features:

* TFVars file inclusion based on `--env` setting (envs/\<ENV\>.tfvars)
* Automatically stores a plan file from `terraform-xp plan`
* Forces `terraform-xp apply` to run only from the generated plan file
* Automatic remote state configuration based on settings in a `remotestate.ini`

### Installation

    curl -fsSL "https://raw.githubusercontent.com/mrjgreen/terraform-xp/master/terraform-xp" > /usr/local/bin/terraform-xp
    chmod +x /usr/local/bin/terraform-xp
    
### Usage

The `--env` flag will set the name of the environment you wish to deploy. If not supplied, the default `global` will be used.

terraform-xp will automaticall include a file matching your env setting, in the location: `envs/\<ENV\>.tfvars`.

Generate a plan file from the directory containing your terraform files. It will place the plan file in `envs/myenv.tfplan`.

    terraform-xp --env myenv plan
    

Apply a plan file. It will look for the plan file in `envs/myenv.tfplan` and fail if the plan file is not present.

    terraform-xp --env myenv apply
    
### Remote State

terraform-xp will look for a file called `remotestate.ini` in your directory and if present, the remote state will be set up before executing the terraform command.

The format should be:

    REMOTE_STATE_BUCKET=my-state-bucket
    REMOTE_STATE_REGION=eu-west-1         # Must match the configured region of your state bucket.
    REMOTE_STATE_PATH=my-new-application  # Key prefix for this app (E.G your app name), do NOT be change once `apply` has ran


This will place your state file in the following location, where \<ENV\> is the value specified in the `--env` flag:

    s3://my-terraform-state-bucket/my-new-application/<ENV>.tfstate

