```
choco install bicep
winget install -e --id Microsoft.Bicep

az login
az --version
az bicep install
az bicep version
az bicep upgrade
>>

mkdir project 
cd project 
touch main.bicep 
code . 

vs code >> extensions >> bicep

az group create --name bicep-rg --location eastus

az group list 

az deployment group create --resource-group bicep-rg --parameters parameters.bicepparam

az deployment group create --name cloudnautic-vm-deployment --resource-group bicep-rg --template-file main.bicep --parameters @parameters.json

```
