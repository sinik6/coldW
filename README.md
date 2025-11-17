# coldW

##  Fontes

* [cite_start]https://bitcoin.org/bitcoin.pdf – white paper original, sempre válido. [cite: 1]
* [cite_start]https://github.com/JoinMarket-Org/joinmarket-clientserver/blob/master/docs/PRIVACY.md – JoinMarket CoinJoin. [cite: 1]
* [cite_start]https://bisq.network/ – trading P2P sem KYC. [cite: 1]
* [cite_start]https://support.bisq.network/ – FAQ técnico. [cite: 2]
* [cite_start]https://unstoppableswap.net/ – Atomic swaps BTC↔XMR (bot 100 % aberto). [cite: 2]
* [cite_start]https://github.com/atomic-wallet/atomic-swap-desk – código aberto para rodar swaps privativos. [cite: 3]
* [cite_start]https://github.com/Rootzoll/raspiblitz/blob/master/README.md – construir um node-off-line em Raspberry Pi. [cite: 3]
* [cite_start]https://reclaimprivacy.github.io/bitcoin-privacy-slides/ – apresentação prática de técnicas. [cite: 4]
* [cite_start]https://github.com/sparrowwallet/sparrow/wiki – Sparrow + Tor + ColdCard ou SeedSigner. [cite: 4]
* [cite_start]https://blog.trezor.io/advanced-coin-control-techniques-7ed6f0d7eb64 – coin control básico. [cite: 4]
* [cite_start]https://conceal.network/blog/btc-xmr-swaps-the-right-way/ – walkthrough de BTC→XMR→BTC sem custódia. [cite: 5]
* [cite_start]https://samouraiwallet.com/whirlpool-stack – documentação Whirlpool CoinJoin. [cite: 5]
* [cite_start]https://code.samourai.io/whirlpool/whirlpool-client-cli – CLI de estagiamento off-line. [cite: 5]
* [cite_start]https://github.com/novalabsxyz/coldcore – ferramenta de tx off-line p/ hardware wallets. [cite: 6]
* [cite_start]https://blog.mempool.space/verify-tx-offline/ – verificação de tx sem gastar sats on-line. [cite: 6]
* [cite_start]https://github.com/jlopp/bitcoin-storage-guide – repositório “long-term-cold-storage”. [cite: 7]
* [cite_start]https://docs.numeraire.org/silent-payment/ – novo tipo de endereço silencioso (receber sem revelar ao p2p network). [cite: 7]
* [cite_start]https://github.com/RCasatta/bitmatrixdetector – verificar se um UTXO já passou por CoinJoin público. [cite: 8]
* [cite_start]https://wiki.featherwallet.org/guides/tailscoinjoin – Tails + Feather (fork da Electrum p/ XMR, mas conceito idêntico). [cite: 9]
* [cite_start]https://nofiat4u.com/how-to-buy-btc-without-kyc-2024/ – resumo prático em PT-BR. [cite: 9]
* [cite_start]https://bitcoinprivacy.org.uk/docs/guide2024.pdf – PDF de 60 páginas revisado até abr/2024. [cite: 10]

---

###  ferramentas  de aquisição anônima

* [cite_start]**Bisq:** instalar Tor integrado, criar conta burner (nickname aleatório, não precisa de email), escolher pagamento via postal money order ou transferência bancária SEPA/E-transfer (nomes falsos e endereço de levantamento em lockers tipo FEDEX Pickup Point). [cite: 10]
* [cite_start]**Hodl Hodl:** similar ao Bisq, suporta Lightning p/ compradores sem conta. [cite: 11]
* [cite_start]**RoboSats:** Telegram bot que atua via Lightning Network, pode ser tornado .onion. [cite: 12]
* [cite_start]**Local Monero MXR → BTC Atomic Swap:** comprar XMR em caixas eletrônicos de Monero (ATM coinFlip ou ATMbitcoinmap) em cash → depois trocar via unstoppableswap.net para BTC endereço silencioso BIP-352 (Silent Payments). [cite: 13]
* [cite_start]**Meet-ups P2P:** encontrar peer em grupos regionais (Mastodon ou Matrix #pt-br-satoshi). [cite: 14]
* [cite_start]**Gift cards sem rastro:** trocar cartões pré-pagos (VISA Gift ou Amex) no Paxful ou AgoraDesk – requer escolher merchants com menos restrições de identificação. [cite: 15]

---

## passo-a-passo com autocustódia irrestrita

### Preparação da estação segura
a. [cite_start]Baixar ISO Tails 5.8 em https://tails.net; [cite: 17] [cite_start]gravar em pendrive 32 GB. [cite: 17]
b. [cite_start]Boot no Tails sem persistência e abrir Tor Browser. [cite: 17]
c. [cite_start]Opcional: air-gapped laptop desconectado permanente para assinatura de tx. [cite: 18]

### carteira
* **SeedSigner (Raspberry Pi Zero + camera):**
    * [cite_start]Imagem .img em https://github.com/seedsigner/seedsigner/releases (verificar signature PGP 0xE21A7A8B). [cite: 19]
    * [cite_start]Gerar seed BIP39 off-line, salvar apenas na placa metal “seedplate”. [cite: 19, 20]
* **Sparrow (desktop, pacote .deb):**
    * [cite_start]Derivar xpub da SeedSigner sem a seed ficar no desktop. [cite: 20]

### Geração do endereço (antes de possuir BTC → novo ciclo)
* [cite_start]**gpg key pair:** `gpg --full-gen-key --expert` (4096 RSA). [cite: 21]
* [cite_start]**Silent Payments:** rodar `bdp-cli newaddress` (repo git clone https://github.com/maaku/bdp) gerando endereço SP1 sem metadados. [cite: 22]

### Bisq ou RoboSats
* [cite_start]**Bisq:** abrir app → Settings → Network → check “use provided Tor". [cite: 23]
* [cite_start]Criar oferta buy BTC/sell fiat com spread > 5 % para atrair vendedores. [cite: 24]
* [cite_start]Receber BTC em hot-sparrow via .onion → fazer primeiro mix Whirlpool. [cite: 25]

### CoinJoin Whirlpool (Samourai)
* [cite_start]Comando headless (pode rodar em VMs isoladas): [cite: 26]
    ```bash
    docker run -it --rm -v whirlpool_data:/data --name whirlpool \
    samouraiwallet/whirlpool-cli start \
    --sccode CK-BY-SC-HERE
    ```
* [cite_start]Ciclos de Premix → Postmix → Remix (custos: 0,01 % fee Samourai + tx mineração). [cite: 26]

### Armazenamento cold storage
* [cite_start]Após ter 100 k sats pré-mix: refundir para wallet criada no SeedSigner com uma tx rodando `bitcoin-cli createrawtransaction` + `signrawtransactionwithkey` (tudo off-line) gravando o PSBT via QR code entre Notebook/Tails e SeedSigner. [cite: 27]
* [cite_start]Reproduzir seed em 3 placas metal (SteelPlates) e enterá-las em locais físicos não associados (GPS lockboxes públicas, por ex., geocaching container). [cite: 28]

###  carteira-cápsula do tempo
* [cite_start]Encriptar seed: [cite: 29]
    ```bash
    openssl enc -aes-256-cbc -salt -in seed_bip39.txt -out seed.enc
    ```
* [cite_start]Dividir encriptado em 2 via Shamir Secret Sharing → utilizar `ssss-split -t 2 -n 3` → distribuir pedaços entre safe-deposit-boxes em diferentes jurisdições. [cite: 29]

## gasto sem linkage
* [cite_start]Sempre usar payjoin (BIP-78) via Sparrow → evita modelos de análise de clustering. [cite: 30]
* [cite_start]Rodar `bitcoin-qt -walletbroadcast=0` on-line e assinar off-line via SD-Card. [cite: 31]
* [cite_start]Trocar pequenas quantias BTC→XMR via unstoppableswap.net quando precisa de fiat cash, finalizando em caixas eletrônicos de XMR ou P2P feito em meetup. [cite: 32]

---
 ### Links extras 

* [cite_start]https://github.com/Anarcho-Crypto-Handbook/anon-btc – projeto colaborativo (pull-requests frequentes). [cite: 32]
* [cite_start]https://github.com/JoinMarket-Org/joinmarket-docs – explicação profunda de Yield-Generators. [cite: 33]
* [cite_start]https://lists.cpunks.org/pipermail/cypherpunks/2024/threads.html – threads recentes sobre Silent Payments, strand-store, SP-tx multicast. [cite: 33]
* [cite_start]https://bitcoindata.science/template-kyc-free-interaction.md – template legal-de-descarte de KYC [cite: 33]
