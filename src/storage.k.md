# Medallion storage model
### Ceiling 

```k
syntax Int ::= "#Ceiling.wards" "[" Int "]" [function]
// -----------------------------------------------
// doc: whether `$0` is an owner of `Vat`
// act: address `$0` is `. == 1 ? authorised : unauthorised`
rule #Ceiling.wards[A] => #hashedLocation("Solidity", 0, A)

syntax Int ::= "#Ceiling.tkn" [function] 
// ----------------------------------------------
// doc: address of the Medallion token contract
// act: 
rule #Ceiling.tkn => 1

syntax Int ::= "#Ceiling.roof" [function]
// -----------------------------------------------
// doc: maximum value of tokenSupply that can be reached
// act: Ceiling ensures the token has no more than . supply
rule #Ceiling.roof => 2
```

### MintLike
TODO: This is probably need

```k
syntax Int ::= "#MintLike.totalSupply" [function]
// ----------------------------------------------
// doc: total supply of a token contract
// act:
rule #MintLike.totalSupply => 0
```

### Medallion
```k
syntax Int ::= "#Medallion.wards" "[" Int "]" [function]
// -----------------------------------------------
// doc: whether `$0` is an owner of `Vat`
// act: address `$0` is `. == 1 ? authorised : unauthorised`
rule #Medallion.wards[A] => #hashedLocation("Solidity", 0, A)

syntax Int ::= "#Medallion.decimals" [function]
// -----------------------------------------------
// doc: decimal places in token balances
// act: The dai token has .` decimal places
rule #Medallion.decimals => 1

syntax Int ::= "#Medallion.name" [function]
// -----------------------------------------------
// doc: the Token name
// act: this token is called .`
rule #Medallion.name => 2

syntax Int ::= "#Medallion.symbol" [function]
// -----------------------------------------------
// doc: the Token symbol
// act: this token has symbol .`
rule #Medallion.symbol => 3

syntax Int ::= "#Medallion.version" [function]
// -----------------------------------------------
// doc: the Token name
// act: this token is version .`
rule #Medallion.version => 4

syntax Int ::= "#Medallion.totalSupply" [function]
// -----------------------------------------------
// doc: the total supply of this token
// act: the total supply is .`
rule #Medallion.totalSupply => 5

syntax Int ::= "#Medallion.balanceOf" "[" Int "]" [function]
// -----------------------------------------------
// doc: the balance of a user
// act: the balance of `$0 is .` us , 
rule #Medallion.balanceOf[A] => #hashedLocation("Solidity", 6, A)

syntax Int ::= "#Medallion.allowance" "[" Int "][" Int "]" [function]
// -----------------------------------------------
// doc: the amount that can be spent on someones behalf
// act: `$1 can spend `.` tokens belonging to `$0`
rule #Medallion.allowance[A][B] => #hashedLocation("Solidity", 7, A B)

syntax Int ::= "#Medallion.nonces" "[" Int "]" [function]
// -----------------------------------------------
// doc: the amount that can be spent on someones behalf
// act: `$1 can spend `.` tokens belonging to `$0`
rule #Medallion.nonces[A] => #hashedLocation("Solidity", 8, A)

syntax Int ::= "#Medallion.DOMAIN_SEPARATOR" [function]
// -----------------------------------------------
// doc: the amount that can be spent on someones behalf
// act: `$1 can spend `.` tokens belonging to `$0`
rule #Medallion.DOMAIN_SEPARATOR => 9

syntax Int ::= "#Medallion.permit_TYPEHASH" [function]
// -----------------------------------------------
// doc: the amount that can be spent on someones behalf
// act: `$1 can spend `.` tokens belonging to `$0`
rule #Medallion.permit_TYPEHASH => 10
```


