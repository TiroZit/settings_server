#  WSL

## Создания моста (чтобы соединить ip wsl)
https://randombytes.substack.com/p/bridged-networking-under-wsl?s=r
1. [Создание моста](https://learn.microsoft.com/en-us/windows-server/virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines?tabs=powershell#create-a-virtual-switch)

2. Создать файл `.wslconfig (C:\Users\TiroZit\.wslconfig)`
``
[wsl2]
networkingMode=bridged
vmSwitch=bridge
``
``
New-VMSwitch -Name bridge  -NetAdapterName <netadapter-name>
``

``
wsl --shutdown
``

