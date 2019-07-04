# Token storage model
### Budget
```k
syntax Int ::= "#Budget.wards" "[" Int "]" [function]
// -----------------------------------------------
// doc: whether `$0` is an owner of `Budget`
// act: address `$0` is `. == 1 ? authorised : unauthorised`
rule #Budget.wards[A] => #hashedLocation("Solidity", 0, A)

syntax Int ::= "#Budget.roof" [function]
// ----------------------------------------------
// doc: address of the Ceiling contract
// act: Ceiling is `$0`
rule #Budget.roof => 1

syntax Int ::= "#Budget.budgets" "[" Int "]" [function]
// -----------------------------------------------
// doc: the available quantity of token for user `$0` to mint 
// act: budget of address `$0` is `.`  
rule #Budget.budgets[A] => #hashedLocation("Solidity", 2, A)
```

### Ceiling
```k
syntax Int ::= "#Ceiling.wards" "[" Int "]" [function]
// -----------------------------------------------
// doc: whether `$0` is an owner of `Ceiling`
// act: address `$0` is `. == 1 ? authorised : unauthorised`
rule #Ceiling.wards[A] => #hashedLocation("Solidity", 0, A)

syntax Int ::= "#Ceiling.tkn" [function]
// ----------------------------------------------
// doc: address of the `Token` contract
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
// doc: whether `$0` is an owner of `Token`
// act: address `$0` is `. == 1 ? authorised : unauthorised`
rule #Token.wards[A] => #hashedLocation("Solidity", 0, A)

syntax Int ::= "#Token.totalSupply" [function]
// -----------------------------------------------
// doc: the total supply of this token
// act: the total supply is .`
rule #Token.totalSupply => 1

syntax Int ::= "#Token.balanceOf" "[" Int "]" [function]
// -----------------------------------------------
// doc: the balance of a user
// act: the balance of `$0 is .` us ,
rule #Token.balanceOf[A] => #hashedLocation("Solidity", 2, A)

syntax Int ::= "#Token.allowance" "[" Int "][" Int "]" [function]
// -----------------------------------------------
// doc: the amount that can be spent on someones behalf
// act: `$1 can spend `.` tokens belonging to `$0`
rule #Token.allowance[A][B] => #hashedLocation("Solidity", 3, A B)

syntax Int ::= "#Token.nonces" "[" Int "]" [function]
// -----------------------------------------------
// doc: the amount that can be spent on someones behalf
// act: `$1 can spend `.` tokens belonging to `$0`
rule #Token.nonces[A] => #hashedLocation("Solidity", 4, A)

syntax Int ::= "#Token.DOMAIN_SEPARATOR" [function]
// -----------------------------------------------
// doc: the amount that can be spent on someones behalf
// act: `$1 can spend `.` tokens belonging to `$0`
rule #Token.DOMAIN_SEPARATOR => 5

syntax Int ::= "#Token.permit_TYPEHASH" [function]
// -----------------------------------------------
// doc: the amount that can be spent on someones behalf
// act: `$1 can spend `.` tokens belonging to `$0`
rule #Token.permit_TYPEHASH => 6
```


