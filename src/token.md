What follows is an executable K specification of the smart contracts for the Centrifuge Token Token. Thanks goes to DappHub for their work on multicollateral dai.

# Budget
The ceiling contract is a ward on a token contract and limits minting to never succeed a set limit on totalSupply.

## Specification of behaviours

### Accessors

```act
behaviour wards of Budget
interface wards(address usr)

types

    May : uint256

storage

    wards[usr] |-> May

iff

    VCallValue == 0

returns May
```


```act
behaviour budgets of Budget
interface budgets(address usr)

types

    Allowance: uint256

storage
    budgets[usr] |-> Allowance => Allowance

iff

    VCallValue == 0

returns Allowance
```


```act
behaviour roof of Budget
interface roof()

types

    Roof: address

storage
  // maximum value of tokenSupply that can be reached
  roof |-> Roof => Roof

iff

    VCallValue == 0

returns Roof
```

### Mutators


#### adding and removing owners

Any owner can add and remove owners.

```act
behaviour rely-diff of Budget
interface rely(address usr)

types

    May   : uint256
    Could : uint256

storage

    wards[CALLER_ID] |-> May
    wards[usr]       |-> Could => 1

iff

    // act: caller is `. ? : not` authorised
    May == 1
    VCallValue == 0

if

    CALLER_ID =/= usr
```

```act
behaviour rely-same of Budget
interface rely(address usr)

types

    May   : uint256

storage

    wards[CALLER_ID] |-> May => 1

iff

    // act: caller is `. ? : not` authorised
    May == 1
    VCallValue == 0

if
    usr == CALLER_ID
```

```act
behaviour deny-diff of Budget
interface deny(address usr)

types

    May   : uint256
    Could : uint256

storage

    wards[CALLER_ID] |-> May
    wards[usr]       |-> Could => 0

iff

    // act: caller is `. ? : not` authorised
    May == 1
    VCallValue == 0

if

    CALLER_ID =/= usr
```

```act
behaviour deny-same of Budget
interface deny(address usr)

types

    Could : uint256

storage

    wards[CALLER_ID] |-> Could => 0

iff

    // act: caller is `. ? : not` authorised
    Could == 1
    VCallValue == 0

if

    CALLER_ID == usr
```


```act
behaviour budget of Budget
interface budget(address usr, uint wad)

types

    May: uint256
    Budget: uint256

storage

    wards[CALLER_ID] |-> May
    budgets[usr] |-> Budget => wad


iff

    // act: caller is `. ? : not` authorised
    May == 1
    VCallValue == 0

```


```act
behaviour mint of Budget
interface mint(address usr, uint wad)

types

    Token         : address Token
    Ceiling       : address Ceiling
    TotalSupply   : uint256
    UsrBal        : uint256
    May           : uint256
    May_medallion : uint256

storage

    wards[CALLER_ID]   |-> May
    roof               |-> Ceiling
    budgets[CALLER_ID] |-> Allow => Allow - wad

storage Ceiling

    wards[ACCT_ID]     |-> May_ceiling
    roof               |-> Roof
    tkn                |-> Token

storage Token

    balanceOf[usr]     |-> UsrBal => UsrBal + wad
    totalSupply        |-> TotalSupply => TotalSupply + wad
    wards[Ceiling]     |-> May_token

iff in range uint256

    Allow - wad
    UsrBal + wad
    TotalSupply + wad

iff

    wad <= Allow
    TotalSupply + wad <= Roof


    // act: caller `. ? : not` authorised
    May == 1
    May_token == 1
    May_ceiling == 1

    VCallValue == 0
    // act: call stack is not too big
    VCallDepth < 1023

calls
    Ceiling.mint

if
    Token =/= Ceiling
```

# Ceiling
The ceiling contract is a ward on a token contract and limits minting to never succeed a set limit on totalSupply.

## Specification of behaviours

### Accessors

```act
behaviour wards of Ceiling
interface wards(address usr)

types

    May : uint256

storage

    wards[usr] |-> May

iff

    VCallValue == 0

returns May
```


```act
behaviour roof of Ceiling
interface roof()

types

    Roof : uint256

storage

    roof |-> Roof

iff

    VCallValue == 0

returns Roof
```

```act
behaviour tkn of Ceiling
interface tkn()

types

    Tkn: address Token

storage
  // maximum value of tokenSupply that can be reached
  tkn |-> Tkn => Tkn

iff

    VCallValue == 0

returns Tkn
```

### Mutators


#### adding and removing owners

Any owner can add and remove owners.

```act
behaviour rely-diff of Ceiling
interface rely(address usr)

types

    May   : uint256
    Could : uint256

storage

    wards[CALLER_ID] |-> May
    wards[usr]       |-> Could => 1

iff

    // act: caller is `. ? : not` authorised
    May == 1
    VCallValue == 0

if

    CALLER_ID =/= usr
```

```act
behaviour rely-same of Ceiling
interface rely(address usr)

types

    May   : uint256

storage

    wards[CALLER_ID] |-> May => 1

iff

    // act: caller is `. ? : not` authorised
    May == 1
    VCallValue == 0

if
    usr == CALLER_ID
```

```act
behaviour deny-diff of Ceiling
interface deny(address usr)

types

    May   : uint256
    Could : uint256

storage

    wards[CALLER_ID] |-> May
    wards[usr]       |-> Could => 0

iff

    // act: caller is `. ? : not` authorised
    May == 1
    VCallValue == 0

if

    CALLER_ID =/= usr
```

```act
behaviour deny-same of Ceiling
interface deny(address usr)

types

    Could : uint256

storage

    wards[CALLER_ID] |-> Could => 0

iff

    // act: caller is `. ? : not` authorised
    Could == 1
    VCallValue == 0

if

    CALLER_ID == usr
```


```act
behaviour mint of Ceiling
interface mint(address usr, uint wad)

types

    Token     : address Token
    Roof          : uint256
    TotalSupply   : uint256
    UsrBal        : uint256
    May           : uint256
    May_medallion : uint256

storage

    wards[CALLER_ID] |-> May
    tkn              |-> Token
    roof             |-> Roof

storage Token
    balanceOf[usr]   |-> UsrBal => UsrBal + wad
    totalSupply      |-> TotalSupply => TotalSupply + wad
    wards[ACCT_ID]   |-> May_medallion

iff in range uint256

    UsrBal + wad
    TotalSupply + wad

iff

    // act: caller `. ? : not` authorised
    May == 1
    May_medallion == 1
    TotalSupply + wad <= Roof

    VCallValue == 0
    // act: call stack is not too big
    VCallDepth < 1024

calls

    Ceiling.adduu
    Token.mint
    Token.totalSupply
    Token.balanceOf
```

### Lemmas
```act
behaviour adduu of Ceiling
interface add(uint256 x, uint256 y) internal
stack
    y : x : JMPTO : WS => JMPTO : x + y : WS
iff in range uint256
    x + y
if
    // TODO: strengthen
    #sizeWordStack(WS) <= 1000
```
# Token

The `Token` contract is the user facing ERC20 contract maintaining the accounting for external Token balances. Most functions are standard for a token with changing supply, but it also notably features the ability to issue approvals for transferFroms based on signed messages, called `Permit`s.

## Specification of behaviours

### Accessors

```act
behaviour wards of Token
interface wards(address usr)

for all

    May : uint256

storage

    wards[usr] |-> May

iff

    VCallValue == 0

returns May
```

```act
behaviour allowance of Token
interface allowance(address holder, address spender)

types

    Allowed : uint256

storage

    allowance[holder][spender] |-> Allowed

iff

    VCallValue == 0

returns Allowed
```

```act
behaviour balanceOf of Token
interface balanceOf(address who)

types

    Balance : uint256

storage

    balanceOf[who] |-> Balance

iff

    VCallValue == 0

returns Balance
```

```act
behaviour totalSupply of Token
interface totalSupply()

types

    Supply : uint256

storage

    totalSupply |-> Supply

iff

    VCallValue == 0

returns Supply
```

```act
behaviour nonces of Token
interface nonces(address who)

types

    Nonce : uint256

storage

    nonces[who] |-> Nonce

iff

    VCallValue == 0

returns Nonce
```

```act
behaviour decimals of Token
interface decimals()

iff

    VCallValue == 0

returns 18
```

```act
behaviour name of Token
interface name()

iff

    VCallValue == 0

returnsRaw #asByteStackInWidthaux(32, 31, 32, #enc(#string("Centrifuge Token")))
```

```act
behaviour version of Token
interface version()

iff

    VCallValue == 0

returnsRaw #asByteStackInWidthaux(32, 31, 32, #enc(#string("1")))
```

```act
behaviour symbol of Token
interface symbol()

iff

    VCallValue == 0

returnsRaw #asByteStackInWidthaux(32, 31, 32, #enc(#string("CENT")))
```

```act
behaviour PERMIT_TYPEHASH of Token
interface PERMIT_TYPEHASH()

iff

    VCallValue == 0

returns keccak(#parseByteStackRaw("Permit(address holder,address spender,uint256 nonce,uint256 expiry,bool allowed)"))
```

```act
behaviour DOMAIN_SEPARATOR of Token
interface DOMAIN_SEPARATOR()

for all

    Dom : uint256

storage

    DOMAIN_SEPARATOR |-> Dom

iff

    VCallValue == 0

returns Dom
```

### Mutators


#### adding and removing owners

Any owner can add and remove owners.

```act
behaviour rely-diff of Token
interface rely(address usr)

for all

    May   : uint256

storage

    wards[CALLER_ID] |-> May
    wards[usr]       |-> _ => 1

iff

    // act: caller is `. ? : not` authorised
    May == 1
    VCallValue == 0

if

    CALLER_ID =/= usr
```

```act
behaviour rely-same of Token
interface rely(address usr)

for all

    May   : uint256

storage

    wards[CALLER_ID] |-> May => 1

iff

    // act: caller is `. ? : not` authorised
    May == 1
    VCallValue == 0

if
    usr == CALLER_ID
```

```act
behaviour deny-diff of Token
interface deny(address usr)

for all

    May   : uint256

storage

    wards[CALLER_ID] |-> May
    wards[usr]       |-> _ => 0

iff

    // act: caller is `. ? : not` authorised
    May == 1
    VCallValue == 0

if

    CALLER_ID =/= usr
```

```act
behaviour deny-same of Token
interface deny(address usr)

for all

    Could : uint256

storage

    wards[CALLER_ID] |-> Could => 0

iff

    // act: caller is `. ? : not` authorised
    Could == 1
    VCallValue == 0

if

    CALLER_ID == usr
```

```act
behaviour adduu of Token
interface add(uint256 x, uint256 y) internal

stack

    y : x : JMPTO : WS => JMPTO : x + y : WS

iff in range uint256

    x + y

if

    // TODO: strengthen
    #sizeWordStack(WS) <= 100
```

```act
behaviour subuu of Token
interface sub(uint256 x, uint256 y) internal

stack

    y : x : JMPTO : WS => JMPTO : x - y : WS

iff in range uint256

    x - y

if

    // TODO: strengthen
    #sizeWordStack(WS) <= 100
```

```act
behaviour transfer-diff of Token
interface transfer(address dst, uint wad)

types

    SrcBal : uint256
    DstBal : uint256

storage

    balanceOf[CALLER_ID] |-> SrcBal => SrcBal - wad
    balanceOf[dst]        |-> DstBal => DstBal + wad

iff in range uint256

    SrcBal - wad
    DstBal + wad

iff

    VCallValue == 0

if
    CALLER_ID =/= dst

returns 1

calls

    Token.adduu
    Token.subuu
```

```act
behaviour transfer-same of Token
interface transfer(address dst, uint wad)

types

    SrcBal : uint256

storage

    balanceOf[CALLER_ID] |-> SrcBal => SrcBal

iff in range uint256

    SrcBal - wad

iff

    VCallValue == 0

if

    CALLER_ID == dst

returns 1

calls

    Token.adduu
    Token.subuu
```

```act
behaviour transferFrom-diff of Token
interface transferFrom(address src, address dst, uint wad)

types

    SrcBal  : uint256
    DstBal  : uint256
    Allowed : uint256

storage

    allowance[src][CALLER_ID] |-> Allowed => #if (src == CALLER_ID or Allowed == maxUInt256) #then Allowed #else Allowed - wad #fi
    balanceOf[src]            |-> SrcBal  => SrcBal  - wad
    balanceOf[dst]            |-> DstBal  => DstBal  + wad

iff in range uint256

    SrcBal - wad
    DstBal + wad

iff
    wad <= Allowed or src == CALLER_ID
    VCallValue == 0

if
    src =/= dst

returns 1

calls

    Token.adduu
    Token.subuu
```

```act
behaviour move-diff of Token
interface move(address src, address dst, uint wad)

types

    SrcBal  : uint256
    DstBal  : uint256
    Allowed : uint256

storage

    allowance[src][CALLER_ID] |-> Allowed => #if (src == CALLER_ID or Allowed == maxUInt256) #then Allowed #else Allowed - wad #fi
    balanceOf[src]            |-> SrcBal  => SrcBal  - wad
    balanceOf[dst]            |-> DstBal  => DstBal  + wad

iff in range uint256

    SrcBal - wad
    DstBal + wad

iff
    wad <= Allowed or src == CALLER_ID
    VCallValue == 0

if
    src =/= dst

calls

    Token.transferFrom-diff
```

```act
behaviour push of Token
interface push(address dst, uint wad)

types

    SrcBal  : uint256
    DstBal  : uint256

storage

    balanceOf[CALLER_ID]      |-> SrcBal  => SrcBal  - wad
    balanceOf[dst]            |-> DstBal  => DstBal  + wad

iff in range uint256

    SrcBal - wad
    DstBal + wad

iff
    VCallValue == 0

if
    CALLER_ID =/= dst

calls

    Token.transferFrom-diff
```

```act
behaviour pull of Token
interface pull(address src, uint wad)

types

    SrcBal  : uint256
    DstBal  : uint256
    Allowed : uint256

storage

    allowance[src][CALLER_ID] |-> Allowed => #if (src == CALLER_ID or Allowed == maxUInt256) #then Allowed #else Allowed - wad #fi
    balanceOf[src]            |-> SrcBal  => SrcBal  - wad
    balanceOf[CALLER_ID]      |-> DstBal  => DstBal  + wad

iff in range uint256

    SrcBal - wad
    DstBal + wad

iff
    wad <= Allowed or src == CALLER_ID
    VCallValue == 0

if
    src =/= CALLER_ID

calls

    Token.transferFrom-diff
```

```act
behaviour transferFrom-same of Token
interface transferFrom(address src, address dst, uint wad)

types

    SrcBal  : uint256
    Allowed : uint256

storage

    allowance[src][CALLER_ID] |-> Allowed => #if (src == CALLER_ID or Allowed == maxUInt256) #then Allowed #else Allowed - wad #fi
    balanceOf[src]            |-> SrcBal  => SrcBal

iff in range uint256

    SrcBal - wad

iff
    wad <= Allowed or src == CALLER_ID
    VCallValue == 0

if
    src == dst

returns 1

calls

    Token.adduu
    Token.subuu
```

```act
behaviour mint of Token
interface mint(address dst, uint wad)

types

    DstBal      : uint256
    TotalSupply : uint256

storage

    wards[CALLER_ID] |-> May
    balanceOf[dst]   |-> DstBal => DstBal + wad
    totalSupply      |-> TotalSupply => TotalSupply + wad

iff in range uint256

    DstBal + wad
    TotalSupply + wad

iff

    May == 1
    VCallValue == 0

calls

    Token.adduu
```

```act
behaviour burn of Token
interface burn(address src, uint wad)

types

    SrcBal      : uint256
    TotalSupply : uint256
    Allowed     : uint256

storage

    allowance[src][CALLER_ID] |-> Allowed => #if (src == CALLER_ID or Allowed == maxUInt256) #then Allowed #else Allowed - wad #fi
    balanceOf[src]            |-> SrcBal => SrcBal - wad
    totalSupply               |-> TotalSupply => TotalSupply - wad

iff in range uint256

    SrcBal - wad
    TotalSupply - wad

iff

    (Allowed == maxUInt256) or (wad <= Allowed) or (src == CALLER_ID)
    VCallValue == 0

calls

    Token.subuu
```


```act
behaviour approve of Token
interface approve(address usr, uint wad)

types

    Allowed : uint256

storage

    allowance[CALLER_ID][usr] |-> Allowed => wad

iff
    VCallValue == 0

returns 1
```

```act
behaviour permit of Token
interface permit(address hodler, address ombudsman, uint256 n, uint256 ttl, bool may, uint8 v, bytes32 r, bytes32 s)

types

    Nonce   : uint256
    Allowed : uint256
    Domain_separator : bytes32

storage

    nonces[hodler]               |-> Nonce => 1 + Nonce
    DOMAIN_SEPARATOR             |-> Domain_separator
    allowance[hodler][ombudsman] |-> Allowed => (#if may == 0 #then 0 #else maxUInt256 #fi)

iff

    hodler == #symEcrec(keccakIntList(#asWord(#parseHexWord("0x19") : #parseHexWord("0x1") : .WordStack) Domain_separator keccakIntList(keccak(#parseByteStackRaw("Permit(address holder,address spender,uint256 nonce,uint256 expiry,bool allowed)")) hodler ombudsman n ttl may)), v, r, s)
    ttl == 0 or TIME <= ttl
    VCallValue == 0
    n == Nonce
    VCallDepth < 1024

if

    #rangeUInt(256, Nonce + 1)
    #rangeUInt(256, ECREC_BAL)
```


