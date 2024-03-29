<div align="center">
  
# Autonomy validator node on Akash Network
# Нода валидатора сети Autonomy, развертка в Akash Network.
  
</div>
  
<div align="center">

![pba](https://user-images.githubusercontent.com/23629420/163564929-166f6a01-a6e2-4412-a4e9-40e54c821f05.png)
| [Akash Network](https://akash.network/) | [Forum Akash Network](https://forum.akash.network/) | 
|:--:|:--:|
___
Before you start - subscribe to our news channels: 

Прежде чем начать - подпишитесь на наши новостные каналы:

| [Discord Akash](https://discord.gg/WR56y8Wt) | [Telegram Akash EN](https://t.me/AkashNW) | [Telegram Akash RU](https://t.me/akash_ru) | [TwitterAkash](https://twitter.com/akashnet_) | [TwitterAkashRU](https://twitter.com/akash_ru) |
|:--:|:--:|:--:|:--:|:--:|

</div>

<div align="center">
  
| [Discord Autonomy](https://discord.gg/KMZqFXg4kj) | [Twitter Autonomy](https://twitter.com/AutonomyHQ) |
|:--:|:--:|
  
</div>

<div align="center">
  
[English version](https://github.com/Dimokus88/Autonomy/blob/main/README.md#deployment-of-the-autonomy-node) | [Русская версия](https://github.com/Dimokus88/Autonomy/blob/main/README.md#%D1%80%D0%B0%D0%B7%D0%B2%D0%B5%D1%80%D1%82%D0%BA%D0%B0-%D0%BD%D0%BE%D0%B4%D1%8B-autonomy)
 
</div>

### Deployment of the Autonomy node.

Deploy the **Autonomy** [node deployment](https://github.com/Dimokus88/Autonomy/blob/main/deploy.yml) with **Akashlytics** ([Instructions for use here](https://github.com/Dimokus88/guides/blob/main/Akashlytics/EN-guide.md)) by installing your the password for the **root** user and the node name in the variables.

<div align="center">
  
![image](https://user-images.githubusercontent.com/23629420/182032552-04d768ff-ac90-4592-9d38-2e00e8fb4455.png)
 
</div>

At this stage, the **RPC** node is deployed. Navigating to the forwarded port **26657** in the ```LEASES``` tab, the node websocket will open, where its up-to-date information will be available.

<div align="center">
  
![image](https://user-images.githubusercontent.com/23629420/182032797-70a74454-75dd-4910-8a30-9a88a1715531.png)

![image](https://user-images.githubusercontent.com/23629420/182032818-069eef95-8242-459f-b503-ad8322261482.png)
 
</div>

If you are interested in a validator node, skip to the next paragraph.

### Run the Autonomy validator

Connect to the running node via **SSH** protocol using port forwarded **22**, user **roo**t and password you set in **deploy.yml**:

![image](https://user-images.githubusercontent.com/23629420/182032966-3fa2ffae-5348-4a2c-a4e8-5d33c57ba320.png)

Run:

```
source ~/.bashrc
```

Open config.toml:

```
nano /root/.autonomy/config/config.toml
```

* Use the down arrow to move the cursor to the **State Sync** section and change the field value ```enable = true``` to ```enable = false```

![image](https://user-images.githubusercontent.com/23629420/182035602-c88af532-321d-4f0b-84b3-32382a8f6fa8.png)

Press the key combination ```ctrl+x``` , then ```'y'``` followed by the ```Enter``` key to save the changes.

Restart the node service with ```sv restart autonomy``` .

Check the synchronization status of the node with ```curl -s localhost:26657/status | jq .result.sync_info.catching_up``` . If the status is **false** - then you can start creating a validator. If the status is **true** - wait for full synchronization.

* Create **Autonomy** wallet or import by **seed** phrase (Create and replace <WALLET_NAME> with your wallet name):

To create a wallet, use the command **(Save the SEED phrase otherwise you risk losing all tokens and access to the wallet!)**

```
autonomy keys add <WALLET_NAME>
```

To import a wallet by seed phrase, use the command:

```
autonomy keys add <WALLET_NAME> --recover
```

* Check the availability of tokens on the balance, to create a validator you need to have more than 1aut account (1aut = 1,000,000 uaut).

```
autonomy query bank balances <ADDRESS>
```

* The command to create a validator looks like this (with automatic delegation 1aut) :

```
autonomy tx staking create-validator --amount="1000000$denom" --pubkey=$($binary tendermint show-validator) --moniker="$MONIKER" --chain-id="$chain" --commission- rate="0.10" --commission-max-rate="0.20" --commission-max-change-rate="0.01" --min-self-delegation="1000000" --gas="auto" --from =<ADDRESS> --fees="5550$denom" -y
```

Check the created validator by replacing <MONIKER> with the name of your validator:

```
autonomy q staking validators -o json | jq .validators[].description.moniker | grep <MONIKER>
```

* Save priv_validator_key.json and node_key.json by copying the contents of the files on your local device:

```
nano /root/.autonomy/config/priv_validator_key.json
```

```
nano /root/.autonomy/config/node_key.json
```  
  
* Delegate the remaining tokens to yourself, after specifying the remaining balance (leave 1,000,000 uat to pay for transaction gas):

```
autonomy tx staking delegate <VALOPER> <amount>uaut --from <ADDRESS> --chain-id $chain --fees 555uaut -y
```

* Collect rewards:

```
autonomy tx distribution withdraw-rewards <VALOPER> --from <ADDRESS> --fees 500uaut --commission --chain-id $chain -y
```
Other commands for managing a node [can be found here](https://github.com/Dimokus88/guides/blob/main/Cosmos%20SDK/COMMAND.MD).

[Back to top](https://github.com/Dimokus88/Autonomy/blob/main/README.md#autonomy-validator-node-on-akash-network)

**Thank you for using Akash Network!**
  
___


### Развертка ноды Autonomy.

Разверните [деплой ноды](https://github.com/Dimokus88/Autonomy/blob/main/deploy.yml) **Autonomy** с помощью **Akashlytics**  ([Инструкция по использованию здесь](https://github.com/Dimokus88/guides/blob/main/Akashlytics/RU-guide.md)) установив свой пароль для **root** пользователя и имя ноды в соответствующих переменных.

<div align="center">
  
![image](https://user-images.githubusercontent.com/23629420/182032552-04d768ff-ac90-4592-9d38-2e00e8fb4455.png)
 
</div>

На данном этапе развернута **RPC** нода. Перейдя на переадресованный порт **26657** во вкладке ```LEASES``` откроется websocket ноды, где будет доступна ее актуальная информация.

<div align="center">
  
![image](https://user-images.githubusercontent.com/23629420/182032797-70a74454-75dd-4910-8a30-9a88a1715531.png)

![image](https://user-images.githubusercontent.com/23629420/182032818-069eef95-8242-459f-b503-ad8322261482.png)
 
</div>

Если вас интересует нода валидатора - перейдите к следующему пункту.

### Запуск валидатора Autonomy

Подключитесь к работающей ноде по протоколу **SSH**, используя переадресованный **22** порт, пользователь **roo**t и пароль заданный вами в **deploy.yml**:

![image](https://user-images.githubusercontent.com/23629420/182032966-3fa2ffae-5348-4a2c-a4e8-5d33c57ba320.png)

Выполните:

```
source ~/.bashrc
```

Откройте config.toml:

```
nano /root/.autonomy/config/config.toml
```

* Стрелкой вниз переместите курсор до раздела **State Sync** и  измените значение поля ```enable = true``` на ```enable = false```

![image](https://user-images.githubusercontent.com/23629420/182035602-c88af532-321d-4f0b-84b3-32382a8f6fa8.png)

Нажмите сочетание клавиш ```ctrl+x``` , затем ```'y'``` и клавишу ```Enter```, сохранив изменения.

Перезапустите службу ноды командой ```sv restart autonomy``` .

Проверьте статус синхронизации ноды командой ```curl -s localhost:26657/status | jq .result.sync_info.catching_up``` . Если статус **false** - то можете приступить к созданию валидатора. Если статус **true** - дождитесь полной синхронизации.

* Создайте кошелек **Autonomy** или импортируйте по **seed** фразе(Придумайте и змените <WALLET_NAME> своим именем кошелька):

Для создания кошелька используйте команду **(Сохраните SEED фразу иначе вы рискуете потерять все токены и доступ к кошельку!)**

```
autonomy keys add <WALLET_NAME>
```

Для импорта кошелька по seed фразе используйте команду:

```
autonomy keys add <WALLET_NAME> --recover
```

* Проверьте наличие токенов на балансе, для создания валидатора необходимо иметь более 1aut счету(1aut = 1 000 000 uaut).

```
autonomy query bank balances <ADDRESS>
```

* Команда создания валидатора выглядит так(с автоматическим делегированием 1aut) :

```
autonomy tx staking create-validator --amount="1000000$denom" --pubkey=$($binary tendermint show-validator) --moniker="$MONIKER"	--chain-id="$chain"	--commission-rate="0.10" --commission-max-rate="0.20" --commission-max-change-rate="0.01" --min-self-delegation="1000000" --gas="auto"	--from=<ADDRESS> --fees="5550$denom" -y
```

Проверьте созданного валидатора заменив <MONIKER> именем вашего валидатора:

```
autonomy q staking validators -o json | jq .validators[].description.moniker | grep <MONIKER>
```
  
* Сохраните priv_validator_key.json и node_key.json скопировав содержимое файлов на вашем локальном устройстве:

```
nano /root/.autonomy/config/priv_validator_key.json
```

```
nano /root/.autonomy/config/node_key.json
```

* Делегируйте на себя оставшиеся токены, предварительно уточнив оставшийся баланс (оставьте 1 000 000 uat для оплаты газа транзакций):

```
autonomy tx staking delegate <VALOPER> <amount>uaut --from <ADDRESS> --chain-id $chain --fees 555uaut -y
```

* Собрать награды:

```
autonomy tx distribution withdraw-rewards <VALOPER> --from <ADDRESS> --fees 500uaut --commission --chain-id $chain -y
```
Другие команды по управлению нодой [можете найти здесь](https://github.com/Dimokus88/guides/blob/main/Cosmos%20SDK/COMMAND.MD).

[К началу](https://github.com/Dimokus88/Autonomy/blob/main/README.md#autonomy-validator-node-on-akash-network)

**Спасибо что воспользовались Akash Network!**
  ___
