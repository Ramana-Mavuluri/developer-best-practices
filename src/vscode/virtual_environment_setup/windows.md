python -m venv .venv
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser -Force
. .\.venv\Scripts\Activate.ps1


$PROFILE | Select-Object *

Invoke-Expression "$(direnv hook pwsh)"

create .envrc file and mention 'layout python'