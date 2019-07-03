# Token storage model
### Ceiling 

```k
syntax Int ::= "#Ceiling.wards" "[" Int "]" [function]
// -----------------------------------------------
// doc: whether `$0` is an owner of `Vat`
// act: address `$0` is `. == 1 ? authorised : unauthorised`
rule #Ceiling.wards[A] => #hashedLocation("Solidity", 0, A)

syntax Int ::= "#Ceiling.tkn" [function] 
// ----------------------------------------------
// doc: address of the Token token contract
// act: 
rule #Ceiling.tkn => 1

syntax Int ::= "#Ceiling.roof" [function]
// -----------------------------------------------
// doc: maximum value of tokenSupply that can be reached
// act: Ceiling ensures the token has no more than . supply
rule #Ceiling.roof => 2
```

### Token
```k
syntax Int ::= "#Token.wards" "[" Int "]" [function]
// -----------------------------------------------
// doc: whether `$0` is an owner of `Vat`
// act: address `$0` is `. == 1 ? authorised : unauthorised`
rule #Token.wards[A] => #hashedLocation("Solidity", 0, A)

syntax Int ::= "#Token.totalSupply" [function]
// -----------------------------------------------
// doc: the total supply of this token
// act: the total supply is .`
rule #Token.totalSupply => 2

syntax Int ::= "#Token.balanceOf" "[" Int "]" [function]
// -----------------------------------------------
// doc: the balance of a user
// act: the balance of `$0 is .` us , 
rule #Token.balanceOf[A] => #hashedLocation("Solidity", 3, A)

syntax Int ::= "#Token.allowance" "[" Int "][" Int "]" [function]
// -----------------------------------------------
// doc: the amount that can be spent on someones behalf
// act: `$1 can spend `.` tokens belonging to `$0`
rule #Token.allowance[A][B] => #hashedLocation("Solidity", 4, A B)

syntax Int ::= "#Token.nonces" "[" Int "]" [function]
// -----------------------------------------------
// doc: the amount that can be spent on someones behalf
// act: `$1 can spend `.` tokens belonging to `$0`
rule #Token.nonces[A] => #hashedLocation("Solidity", 5, A)

syntax Int ::= "#Token.DOMAIN_SEPARATOR" [function]
// -----------------------------------------------
// doc: the amount that can be spent on someones behalf
// act: `$1 can spend `.` tokens belonging to `$0`
rule #Token.DOMAIN_SEPARATOR => 6

syntax Int ::= "#Token.permit_TYPEHASH" [function]
// -----------------------------------------------
// doc: the amount that can be spent on someones behalf
// act: `$1 can spend `.` tokens belonging to `$0`
rule #Token.permit_TYPEHASH => 7
```


