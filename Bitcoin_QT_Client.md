## Console commands

https://en.bitcoin.it/wiki/How_to_import_private_keys_v7%2B

#### unlock wallet.dat (times in seconds)

    walletpassphrase "YourLongPassphrase" 600

#### import address via private key

    importprivkey yourPrivateKeyInWalletImportFormat "TheLabelThatIWant"

#### print private key of an address

    dumpprivkey publickeyhere

#### print existing keys (public ones)

    listreceivedbyaddress 0 true
    listaddressgroupings

#### print info about the running client

    getinfo
