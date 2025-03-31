# TERRAFORM CHEAT SHEET 

### Basic commands 

|**DESCRIPTION** | **COMMAND** |
|---|---|
| Show version | terraform version |
| Download and update modules | terraform get -update=true |
| shom terraformt terminal | terraform console |
| Create a diagram os tf dependencias (great for visualization) | terraform graph | dot -Tpng > grap.png |
| Format tf files to HCL standarts | terraform fmt |
| Validate terraform syntax | terraform validade |
| Atcivate terraform auto-completion | terraform -install-autocomplete |
| show providers in use | terraform providers |

### Initialize 
|**DESCRIPTION** | **COMMAND** |
|---|---|
| Initialize terraform | terraform init |
| Initialize terraform without plugins | terraform init -get-plugins=false |
| Upgrade plugins and modules at initialization | terraform init upgrade |
| Change state lock timeout (default zero seconds) | terraform init -lock-timeout=120s |

###  Plan Step
|**DESCRIPTION** | **COMMAND** |
|---|---|
| Create a plan with diff between code and state | terraform plan |
| Output a plan file for reference during apply | terraform plan -out <tfplan-file> |
| Output a plan to show effect of terraform destroy | terraform plan -destroy |
| Target a specific resource for deployment | terraform plan -target=Resource | 

###  Apply Step
|**DESCRIPTION** | **COMMAND** |
|---|---|
|Apply the state of terraform code | terraform apply |
|Specify a previously generated plan to apply |terraform apply <tfplan-file> | 
|Enable auto-approval or automation | terraform apply -auto-approve |

###  Destroy Step
|**DESCRIPTION** | **COMMAND** |
|---|---|
| Destroy resources managed by terraform state| terraform destroy |
| Enable auto-approval or automation **CAUTION**| terraform destroy -auto-approve | 

###  Output Step
|**DESCRIPTION** | **COMMAND** |
|---|---|
| List available outputs | terraform output |
| Output a specific value |terraform output NAME|

###  Output Step
|**DESCRIPTION** | **COMMAND** |
|---|---|
|List all resources in terraform state |terraform state list |
|show details about a specific resource|terraform state show ADDRESS|
|Import a manually created resource into state|terraform state import ADDRESS ID|
|Track an existing resource in state under new name|terraform state mv SOURCE DESTINATION|
|Pull state and save to a local file|terraform state pull > <tfstate-file>|
|push state to a remote location|terraform state push PATH|
|Replace a resource provider|terraform state replace-provider A B|
|Taint a resource to force redeployment on apply|terraform taint ADDRESS|
|Untaint a prevoiusly tainted resource|terraform untaint ADDRESS|