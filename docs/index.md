<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

### Table of Contents

-   [TransactionStateManager][1]
    -   [Parameters][2]
    -   [generateTxMeta][3]
        -   [Parameters][4]
    -   [getTxList][5]
    -   [getFullTxList][6]
    -   [getUnapprovedTxList][7]
    -   [getApprovedTransactions][8]
        -   [Parameters][9]
    -   [getPendingTransactions][10]
        -   [Parameters][11]
    -   [getConfirmedTransactions][12]
        -   [Parameters][13]
    -   [addTx][14]
        -   [Parameters][15]
    -   [getTx][16]
        -   [Parameters][17]
    -   [updateTx][18]
        -   [Parameters][19]
    -   [updateTxParams][20]
        -   [Parameters][21]
    -   [validateTxParams][22]
        -   [Parameters][23]
    -   [getFilteredTxList][24]
        -   [Parameters][25]
    -   [getTxsByMetaData][26]
        -   [Parameters][27]
    -   [getTxStatus][28]
        -   [Parameters][29]
    -   [setTxStatusRejected][30]
        -   [Parameters][31]
    -   [setTxStatusUnapproved][32]
        -   [Parameters][33]
    -   [setTxStatusApproved][34]
        -   [Parameters][35]
    -   [setTxStatusSigned][36]
        -   [Parameters][37]
    -   [setTxStatusSubmitted][38]
        -   [Parameters][39]
    -   [setTxStatusConfirmed][40]
        -   [Parameters][41]
    -   [setTxStatusDropped][42]
        -   [Parameters][43]
    -   [setTxStatusFailed][44]
        -   [Parameters][45]
    -   [wipeTransactions][46]
        -   [Parameters][47]
    -   [\_setTxStatus][48]
        -   [Parameters][49]
    -   [\_saveTxList][50]
        -   [Parameters][51]
## TransactionStateManager

[index.js:31-452][69]

**Extends EventEmitter**

TransactionStateManager is responsible for the state of a transaction and
storing the transaction
it also has some convenience methods for finding subsets of transactions

STATUS METHODS
<br>statuses:
<br>   - `'unapproved'` the user has not responded
<br>   - `'rejected'` the user has responded no!
<br>   - `'approved'` the user has approved the tx
<br>   - `'signed'` the tx is signed
<br>   - `'submitted'` the tx is sent to a server
<br>   - `'confirmed'` the tx has been included in a block.
<br>   - `'failed'` the tx failed for some reason, included on tx data.
<br>   - `'dropped'` the tx nonce was already used

### Parameters

-   `opts` **[Object][70]** {object}
    -   `opts.initState` **[object][70]** initial transactions list with the key transaction {array} (optional, default `{transactions:[]}`)
    -   `opts.txHistoryLimit` **[number][71]?** limit for how many finished
        transactions can hang around in state
    -   `opts.getNetwork` **[function][72]** return network number

### generateTxMeta

[index.js:47-57][73]

#### Parameters

-   `opts`  {object} - the object to use when overwriting defaults

Returns **txMeta** the default txMeta object

### getTxList

[index.js:62-66][74]

Returns **[array][75]** of txMetas that have been filtered for only the current network

### getFullTxList

[index.js:71-73][76]

Returns **[array][75]** of all the txMetas in store

### getUnapprovedTxList

[index.js:78-84][77]

Returns **[array][75]** the tx list whos status is unapproved

### getApprovedTransactions

[index.js:91-95][78]

#### Parameters

-   `address`  {string} - hex prefixed address to sort the txMetas for [optional]

Returns **[array][75]** the tx list whos status is approved if no address is provide
returns all txMetas who's status is approved for the current network

### getPendingTransactions

[index.js:102-106][79]

#### Parameters

-   `address`  {string} - hex prefixed address to sort the txMetas for [optional]

Returns **[array][75]** the tx list whos status is submitted if no address is provide
returns all txMetas who's status is submitted for the current network

### getConfirmedTransactions

[index.js:113-117][80]

#### Parameters

-   `address`  {string} - hex prefixed address to sort the txMetas for [optional]

Returns **[array][75]** the tx list whos status is confirmed if no address is provide
returns all txMetas who's status is confirmed for the current network

### addTx

[index.js:128-161][81]

Adds the txMeta to the list of transactions in the store.
if the list is over txHistoryLimit it will remove a transaction that
is in its final state
it will allso add the key `history` to the txMeta with the snap shot of the original
object

#### Parameters

-   `txMeta`  {Object}

Returns **[object][70]** the txMeta

### getTx

[index.js:167-170][82]

#### Parameters

-   `txId`  {number}

Returns **[object][70]** the txMeta who matches the given id if none found
for the network returns undefined

### updateTx

[index.js:177-201][83]

updates the txMeta in the list and adds a history entry

#### Parameters

-   `txMeta`  {Object} - the txMeta to update
-   `note`  {string} - a note about the update for history

### updateTxParams

[index.js:210-214][84]

merges txParams obj onto txMeta.txParams
use extend to ensure that all fields are filled

#### Parameters

-   `txId`  {number} - the id of the txMeta
-   `txParams`  {object} - the updated txParams

### validateTxParams

[index.js:220-234][85]

validates txParams members by type

#### Parameters

-   `txParams`  {object} - txParams to validate

### getFilteredTxList

[index.js:262-268][86]

#### Parameters

-   `opts`  {object} -  an object of fields to search for eg:<br>
    let <code>thingsToLookFor = {<br>
    to: '0x0..',<br>
    from: '0x0..',<br>
    status: 'signed',<br>
    err: undefined,<br>
    }<br></code>
-   `initialList`   (optional, default `this.getTxList()`)

Returns **any** a {array} of txMeta with all
options matching

### getTxsByMetaData

[index.js:277-285][87]

#### Parameters

-   `key`  {string} - the key to check
-   `value`  the value your looking for
-   `txList`  {array} - the list to search. default is the txList
    from txStateManager#getTxList (optional, default `this.getTxList()`)

Returns **[array][75]** a list of txMetas who matches the search params

### getTxStatus

[index.js:293-296][88]

#### Parameters

-   `txId`  {number} - the txMeta Id

Returns **[string][89]** the status of the tx.

### setTxStatusRejected

[index.js:302-305][90]

should update the status of the tx to 'rejected'.

#### Parameters

-   `txId`  {number} - the txMeta Id

### setTxStatusUnapproved

[index.js:311-313][91]

should update the status of the tx to 'unapproved'.

#### Parameters

-   `txId`  {number} - the txMeta Id

### setTxStatusApproved

[index.js:318-320][92]

should update the status of the tx to 'approved'.

#### Parameters

-   `txId`  {number} - the txMeta Id

### setTxStatusSigned

[index.js:326-328][93]

should update the status of the tx to 'signed'.

#### Parameters

-   `txId`  {number} - the txMeta Id

### setTxStatusSubmitted

[index.js:335-340][94]

should update the status of the tx to 'submitted'.
and add a time stamp for when it was called

#### Parameters

-   `txId`  {number} - the txMeta Id

### setTxStatusConfirmed

[index.js:346-348][95]

should update the status of the tx to 'confirmed'.

#### Parameters

-   `txId`  {number} - the txMeta Id

### setTxStatusDropped

[index.js:354-356][96]

should update the status of the tx to 'dropped'.

#### Parameters

-   `txId`  {number} - the txMeta Id

### setTxStatusFailed

[index.js:365-376][97]

should update the status of the tx to 'failed'.
and put the error on the txMeta

#### Parameters

-   `txId`  {number} - the txMeta Id
-   `err`  {erroObject} - error object

### wipeTransactions

[index.js:383-393][98]

Removes transaction from the given address for the current network
from the txList

#### Parameters

-   `address`  {string} - hex string of the from address on the txParams to remove

### \_setTxStatus

[index.js:416-437][99]

#### Parameters

-   `txId`  {number} - the txMeta Id
-   `status`  {string} - the status to set on the txMeta

### \_saveTxList

[index.js:444-446][100]

Saves the new/updated txList.

#### Parameters

-   `transactions`  {array} - the list of transactions to save


[1]: #transactionstatemanager

[2]: #parameters

[3]: #generatetxmeta

[4]: #parameters-1

[5]: #gettxlist

[6]: #getfulltxlist

[7]: #getunapprovedtxlist

[8]: #getapprovedtransactions

[9]: #parameters-2

[10]: #getpendingtransactions

[11]: #parameters-3

[12]: #getconfirmedtransactions

[13]: #parameters-4

[14]: #addtx

[15]: #parameters-5

[16]: #gettx

[17]: #parameters-6

[18]: #updatetx

[19]: #parameters-7

[20]: #updatetxparams

[21]: #parameters-8

[22]: #validatetxparams

[23]: #parameters-9

[24]: #getfilteredtxlist

[25]: #parameters-10

[26]: #gettxsbymetadata

[27]: #parameters-11

[28]: #gettxstatus

[29]: #parameters-12

[30]: #settxstatusrejected

[31]: #parameters-13

[32]: #settxstatusunapproved

[33]: #parameters-14

[34]: #settxstatusapproved

[35]: #parameters-15

[36]: #settxstatussigned

[37]: #parameters-16

[38]: #settxstatussubmitted

[39]: #parameters-17

[40]: #settxstatusconfirmed

[41]: #parameters-18

[42]: #settxstatusdropped

[43]: #parameters-19

[44]: #settxstatusfailed

[45]: #parameters-20

[46]: #wipetransactions

[47]: #parameters-21

[48]: #_settxstatus

[49]: #parameters-22

[50]: #_savetxlist

[51]: #parameters-23


[69]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L31-L452 "Source code on GitHub"

[70]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Object

[71]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Number

[72]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/function

[73]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L47-L57 "Source code on GitHub"

[74]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L62-L66 "Source code on GitHub"

[75]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Array

[76]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L71-L73 "Source code on GitHub"

[77]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L78-L84 "Source code on GitHub"

[78]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L91-L95 "Source code on GitHub"

[79]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L102-L106 "Source code on GitHub"

[80]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L113-L117 "Source code on GitHub"

[81]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L128-L161 "Source code on GitHub"

[82]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L167-L170 "Source code on GitHub"

[83]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L177-L201 "Source code on GitHub"

[84]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L210-L214 "Source code on GitHub"

[85]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L220-L234 "Source code on GitHub"

[86]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L262-L268 "Source code on GitHub"

[87]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L277-L285 "Source code on GitHub"

[88]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L293-L296 "Source code on GitHub"

[89]: https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/String

[90]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L302-L305 "Source code on GitHub"

[91]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L311-L313 "Source code on GitHub"

[92]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L318-L320 "Source code on GitHub"

[93]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L326-L328 "Source code on GitHub"

[94]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L335-L340 "Source code on GitHub"

[95]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L346-L348 "Source code on GitHub"

[96]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L354-L356 "Source code on GitHub"

[97]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L365-L376 "Source code on GitHub"

[98]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L383-L393 "Source code on GitHub"

[99]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L416-L437 "Source code on GitHub"

[100]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/index.js#L444-L446 "Source code on GitHub"

[101]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/lib/tx-state-history-helper.js#L4-L9 "Source code on GitHub"

[102]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/lib/util.js#L9-L15 "Source code on GitHub"

[103]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/lib/tx-state-history-helper.js#L39-L48 "Source code on GitHub"

[104]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/lib/tx-state-history-helper.js#L54-L57 "Source code on GitHub"

[105]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/lib/tx-state-history-helper.js#L63-L69 "Source code on GitHub"

[106]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/lib/util.js#L19-L27 "Source code on GitHub"

[107]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/lib/util.js#L47-L60 "Source code on GitHub"

[108]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/lib/util.js#L66-L69 "Source code on GitHub"

[109]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/lib/util.js#L75-L86 "Source code on GitHub"

[110]: https://github.com/MetaMask/transaction-state-manager/blob/f0331e04a4f5a525d86307481f63d418a4e9d159/lib/util.js#L91-L98 "Source code on GitHub"
