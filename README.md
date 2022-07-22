
# Руководство по запуску Connext Router



1.Устанавливаем Docker

```
sudo apt-get install ca-certificates curl gnupg lsb-release -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io -y
```
2. Устанавливаем Doscker Compose
```
mkdir -p ~/.docker/cli-plugins/
curl -SL https://github.com/docker/compose/releases/download/v2.2.3/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
chmod +x ~/.docker/cli-plugins/docker-compose
sudo chown $USER /var/run/docker.sock
```
3. Клонируем репозиторий
```
cd ~
git clone https://github.com/connext/nxtp-router-docker-compose.git
cd nxtp-router-docker-compose
git checkout amarok
```


4. Настройка конфига
Создайте .env файла на основе env.example .

5. Укажите переменные
```
ROUTER_VERSION
ROUTER_EXTERNAL_PORT 
GRAFANA_EXTERNAL_PORT 
LOGDNA_KEY 
```
6. Используйте данные команды
```
mv .env.example .env
mv key.example.yaml key.yaml
```


7. Добавьте Ваш приватный ключ в key.yaml.
Используйте Метамаск для этого

8. Создайте конфиг роутера
Создайте конфиг роутера на основе config.example.json file. Используйте эти данные.

```
sequencerUrl - The URL of the Sequencer node.
redis - The Redis instance to use.
server - Internal HTTP server config (adminToken).
chains - Add your desired chains, assets, and provider URLs. Use domain mappings instead of chainIds. For more domain ids of chains, please check https://raw.githubusercontent.com/connext/chaindata/main/crossChain.json . Make sure you use multiple providers for each chain! Example with the current testnet assets:
nano config.toml
{
  "chains": {
    "1111": {
      "assets": [
        {
          "address": "0xB7b1d3cC52E658922b2aF00c5729001ceA98142C",
          "name": "TEST"
        }
      ],
      "providers": ["https://eth-rinkeby.alchemyapi.io/v2/Bi4KoxT0rgjHsmVvMRtdY_9rwrTsdY2h", "https://rpc.ankr.com/eth_rinkeby"]
    },
    "2221": {
      "assets": [
        {
          "address": "0xB5AabB55385bfBe31D627E2A717a7B189ddA4F8F",
          "name": "TEST"
        }
      ],
      "providers": ["https://eth-kovan.alchemyapi.io/v2/eEcEgHdAt3fkZkRmzbaI-Sv_1Gw6OxrB"]
    }
  },
  "logLevel": "debug",
  "mnemonic": "",
  "sequencerUrl": "https://sequencer.testnet.connext.ninja",
  "server": { "adminToken": "blahblahblah" },
  "environment": "production"
}
```

9.Запустите  Docker
```
docker-compose up -d
```

10. Проверьте логи
```
docker logs --follow --tail 100 router
```
Если необходимо сделайте рестарт
```
docker-compose restart
```
