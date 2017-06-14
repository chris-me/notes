## Konsolenbefehle

https://en.bitcoin.it/wiki/How_to_import_private_keys_v7%2B

#### wallet.dat entsperren (Zeit in Sekunden)

    walletpassphrase "YourLongPassphrase" 600

#### Adresse mithilfe von privatem Schlüssel importieren

    importprivkey yourPrivateKeyInWalletImportFormat "TheLabelThatIWant"

#### Privaten Key von Adresse ausgeben:\'''

    dumpprivkey publickeyhere

#### Im wallet vorhandene Keys auflisten (listet public keys)

    listreceivedbyaddress 0 true
    listaddressgroupings

#### Informationen über den laufenden Client ausgeben

    getinfo
