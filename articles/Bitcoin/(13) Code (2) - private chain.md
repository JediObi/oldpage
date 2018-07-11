### **Code (2) - private chain**



The most concise way to create private chain is that change the config parameters of the core code. 
You can see these config parameters in ```chainparams.cpp```. 

Now we start to create a private chain. 



## (1) modify the genesis block

 #### 1) modify the coinbase info of genesis block

+ ##### The original code 

```CreateGenesisBlock``` to create genesis block. 

```
static CBlock CreateGenesisBlock(uint32_t nTime, uint32_t nNonce, uint32_t nBits, int32_t nVersion, const CAmount& genesisReward)
{
    const char* pszTimestamp = "The Times 03/Jan/2009 Chancellor on brink of second bailout for banks";
    const CScript genesisOutputScript = CScript() << ParseHex("04678afdb0fe5548271967f1a67130b7105cd6a828e03909a67962e0ea1f61deb649f6bc3f4cef38c4f35504e51ec112de5c384df7ba0b8d578a4c702b6bf11d5f") << OP_CHECKSIG;
    return CreateGenesisBlock(pszTimestamp, genesisOutputScript, nTime, nNonce, nBits, nVersion, genesisReward);
}
```

+ ##### the places need to modify

1. the code of create genesis block 
```genesis = CreateGenesisBlock(1231006505, 2083236893, 0x1d00ffff, 1, 50 * COIN);``` 
1231006505 nTime, the time of the block 

2. set mining difficulty, the lowest difficulty 
```consensus.powLimit = uint256S("00000000ffffffffffffffffffffffffffffffffffffffffffffffffffffffff");``` 

3. total amount of coins 
```consensus.nSubsidyHalvingInterval = 210000;``` 

4. change the initial reward 
```genesis = CreateGenesisBlock(1231006505, 2083236893, 0x1d00ffff, 1, 50 * COIN);``` 
50 * COIN is the inital reward. 

5. change the time period of difficulty changing 

```
consensus.nPowTargetTimespan = 14 * 24 * 60 * 60; // two weeks to change difficulty
consensus.nPowTargetSpacing = 10 * 60;  // the time interval of per block.
consensus.nMinerConfirmationWindow = 2016; // nPowTargetTimespan / nPowTargetSpacing
```



## (2) change network protocol magic number

You can change it to any other byte. 

```
pchMessageStart[0] = 0x0b;
pchMessageStart[1] = 0x11;
pchMessageStart[2] = 0x09;
pchMessageStart[3] = 0x07;
```



## (3) change listen port

chainparams.cpp 

```
nDefaultPort = 18333;
```

chianparamsbase.cpp 

```
nRPCPort=8332;
```



## (4) change the definition of seed connection

```
vSeeds.emplace_back("seed.bitcoin.sipa.be"); // Pieter Wuille, only supports x1, x5, x9, and xd
vSeeds.emplace_back("dnsseed.bluematt.me"); // Matt Corallo, only supports x9
vSeeds.emplace_back("dnsseed.bitcoin.dashjr.org"); // Luke Dashjr
vSeeds.emplace_back("seed.bitcoinstats.com"); // Christian Decker, supports x1 - xf
vSeeds.emplace_back("seed.bitcoin.jonasschnelli.ch"); // Jonas Schnelli, only supports x1, x5, x9, and xd
vSeeds.emplace_back("seed.btc.petertodd.org"); // Peter Todd, only supports x1, x5, x9, and xd
vSeeds.emplace_back("seed.bitcoin.sprovoost.nl"); // Sjors Provoost
```


## (5) change the prefixes of bitcoin

```
base58Prefixes[PUBKEY_ADDRESS] = std::vector<unsigned char>(1,0);
base58Prefixes[SCRIPT_ADDRESS] = std::vector<unsigned char>(1,5);
base58Prefixes[SECRET_KEY] =     std::vector<unsigned char>(1,128);
base58Prefixes[EXT_PUBLIC_KEY] = {0x04, 0x88, 0xB2, 0x1E};
base58Prefixes[EXT_SECRET_KEY] = {0x04, 0x88, 0xAD, 0xE4};
```



## (6) coinbase maturity confirmations

```
/** Coinbase transaction outputs can only be spent after this number of new blocks (network rule) */
static const int COINBASE_MATURITY = 100;
```



## (7) change checkpoint

You can use the genesis block's check point. check point is used to prevent forks. 
You can start bitcoind once and get the genesis from logs. 

checkpointData = {
            {}
} 
the original code.

```
checkpointData = {
            {
                { 11111, uint256S("0x0000000069e244f73d78e8fd29ba2fd2ed618bd6fa2ee92559f542fdb26e7c1d")},
                { 33333, uint256S("0x000000002dd5588a74784eaa7ab0507a18ad16a236e7b1ce69f00d7ddfb5d0a6")},
                { 74000, uint256S("0x0000000000573993a3c9e41ce34471c079dcf5f52a0e824a81e7f953b8661a20")},
                {105000, uint256S("0x00000000000291ce28027faea320c8d2b054b2e0fe44a773f3eefb151d6bdc97")},
                {134444, uint256S("0x00000000000005b12ffd4cd315cd34ffd4a594f430ac814c91184a0d42d2b0fe")},
                {168000, uint256S("0x000000000000099e61ea72015e79632f216fe6cb33d7899acb35b75c8303b763")},
                {193000, uint256S("0x000000000000059f452a5f7340de6682a977387c17010ff6e6c3bd83ca8b1317")},
                {210000, uint256S("0x000000000000048b95347e83192f69cf0366076336c639f9b7228e9ba171342e")},
                {216116, uint256S("0x00000000000001b4f4b433e81ee46494af945cf96014816a4e2370f11b23df4e")},
                {225430, uint256S("0x00000000000001c108384350f74090433e7fcf79a606b8e797f065b130575932")},
                {250000, uint256S("0x000000000000003887df1f29024b06fc2200b55f8af8f35453d7be294df2d214")},
                {279000, uint256S("0x0000000000000001ae8c72a0b0c301f67e3afca10e819efa9041e458e9bd7e40")},
                {295000, uint256S("0x00000000000000004d9b4ef50f0f9d686fd69db2e03af35a100370c64632a983")},
            }
        };
```



## (8) change the scriptPubKey

```
const CScript genesisOutputScript = CScript() << ParseHex("04678afdb0fe5548271967f1a67130b7105cd6a828e03909a67962e0ea1f61deb649f6bc3f4cef38c4f35504e51ec112de5c384df7ba0b8d578a4c702b6bf11d5f") << OP_CHECKSIG;
```



## (9) change the mining algorithm

Bitcoind's original mining algorithm is in miner.cpp ScanHash. 
ScanHash will keep the block info's first 5 fields (version, prev_block, merkle_root, timestamp, bits), totally 76 bytes, unchanged and via ergodic Nonce and then concat the 5 fields and Nonce to compute hash. 
If the first K bytes is 0, then the hash is effective, mining susccess. 

mining.cpp

```
UniValue generateBlocks(std::shared_ptr<CReserveScript> coinbaseScript, int nGenerate, uint64_t nMaxTries, bool keepScript)
{
	
    static const int nInnerLoopCount = 0x10000;
    int nHeightEnd = 0;
    int nHeight = 0;

    {   // Don't keep cs_main locked
        LOCK(cs_main);
        nHeight = chainActive.Height();
        nHeightEnd = nHeight+nGenerate;
    }
    unsigned int nExtraNonce = 0;
    UniValue blockHashes(UniValue::VARR);
    while (nHeight < nHeightEnd && !ShutdownRequested())
    {
	//根据之前的参数创建一个没有hash的区块
        std::unique_ptr<CBlockTemplate> pblocktemplate(BlockAssembler(Params()).CreateNewBlock(coinbaseScript->reserveScript));
        if (!pblocktemplate.get())
            throw JSONRPCError(RPC_INTERNAL_ERROR, "Couldn't create new block");
	// 把模板创建的区块 赋值给标准的区块
        CBlock *pblock = &pblocktemplate->block;
//获取Nonce
        {
            LOCK(cs_main);
            IncrementExtraNonce(pblock, chainActive.Tip(), nExtraNonce);
        }
//Nonce重复执行，同时验证工作量，通过pblock GetHash()获得当前block的hash值。
        while (nMaxTries > 0 && pblock->nNonce < nInnerLoopCount && !CheckProofOfWork(pblock->GetHash(), pblock->nBits, Params().GetConsensus())) {
            ++pblock->nNonce;
            --nMaxTries;
        }
        if (nMaxTries == 0) {
            break;
        }
        if (pblock->nNonce == nInnerLoopCount) {
            continue;
        }
        std::shared_ptr<const CBlock> shared_pblock = std::make_shared<const CBlock>(*pblock);
        if (!ProcessNewBlock(Params(), shared_pblock, true, nullptr))
            throw JSONRPCError(RPC_INTERNAL_ERROR, "ProcessNewBlock, block not accepted");
        ++nHeight;
        blockHashes.push_back(pblock->GetHash().GetHex());

        //mark script as important because it was used at least for one coinbase output if the script came from the wallet
        if (keepScript)
        {
            coinbaseScript->KeepScript();
        }
    }
    return blockHashes;
}
```

通过为CBlock对象各字段赋值，然后调用GetHash()通过底层的CHash256和CSHA256等类来计算hash，然后验证工作量。 