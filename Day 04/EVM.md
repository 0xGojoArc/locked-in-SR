##### [[EVM]]

### Transactions

- They are atomic operation
    
- It is either all(complete) or nothing(zero)
    
- Order is sequential, with no overlapping.
    
- Multiple actors can submit the transaction at the same time, but the order is not guaranteed.
    

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXc_bMifDu2reuOJql9nLaGjSyK3Hz-2BIxTUvWhlb3KY01jbKMSy07qH45xoiF3ZYC7Rzw1PMROV5WWhoixaLWc8oQMiCSVMj9t80xiPdxcRlEpoheQz_bnMJqhSJCluPP5K13Ab_XsWIYaUN8Y6xy_lAsr?key=oZ3gdgHh9pSWu4oJzdf49w)

- EVM is runtime env for SC bytecode in Ethereum
    
- EVM is a stack-based architecture
    
- It has three main parts: 
    

1. Virtual ROM - immutable EVM bytecode
    
2. Machine State - volatile
    

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe3fS-z4up7_ck8-EJglD1KiymV0U48Bo8Q_1ruXCkSB1XRXi6_77UxzKEEPAN2tGZvtY_9F0_WStv4wrxjgOXWEcplshxh1hpRr2mMQLauPTVw9Oaf4_HMYmPMzURAwGBlpIml235dn3wVoDLZLcSc9ZvQ?key=oZ3gdgHh9pSWu4oJzdf49w)

3. World State - persistent Account storage
    

- In machine State, the Stack has 1024 elements of 256 bits
    
- In Memory, volatile and byte addressing linear memory
    
- Account storage 256 bits - 256 bits key value stores
    
- All operations are performed on the Stack.
    
- Instructions like POP/PUSH/COPY/SWAP access Stack
    

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcxrOtBSvwy5WVttVrqS9OCgoD7YUIT_RcZ2Tkjcqgt8rty8ASgTA1xRHKMWE8Fo_JbCb8nITcqDbLwTB1JnMSiQWcTkv7t6lObeQZ60PDimum9P-zSwk6lzHJ5qldFHaIYQxryd7N7sAH1DF0Tk105X7k?key=oZ3gdgHh9pSWu4oJzdf49w)

- Memory stores 256-bit or 8-bit but loads 256-bit
    
- Memory is linear and can be addressed at byte code
    
- Memory is accessed with MSTORE/MSTORE8/MLOAD instructions
    
- Memory’s all locations are initially defined as zero  
    ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe9wOmEIBOOVyWIWt5rn5Kj_703UrYFrT8IFxpef3YGUmYGnXveR5GdcZByw5FwFcoSrkjf3aDgMW_L6SULhbJjxH665FSX28Zj42GinYkgmNSkpQbGbjsnNEvYRx48a3XgFTlHKjZvdJqtp-kq4G5qBkbb?key=oZ3gdgHh9pSWu4oJzdf49w)
    
- Account Storage is key-value store mapping 256 bit to 256 bit
    
- Storage access with STORE/SLOAD instructions
    
- Storages all locations are initially defined as zero  
    ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXccICDbub61ZawXE8YOWSm7HR3k6U3gNTwY92NtZaxEV_azETyWIgR-JINjQgkHHlB6UJmjJ5e6iwbTeFpk4_ayMEHjFxIkbHGyJYikHgkJLTaSclVajyZAap1gyMQH4r77FAYhwhF_wQSaXQgPcjy59B_H?key=oZ3gdgHh9pSWu4oJzdf49w)
    
- EVM code is a byte code
    
- EVM code’s instructions are used to perform operations received by the PC, that performs POP/PUSH/… operations stack in two directional ways to Stack, Stack to Memory and Account Storage  
    ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXf0iFynqGOoHATQXbngDonEya21kcIdMrUtrnHTtoJ_p5d16XtnIJ1CFMuAn_VAx-PAS_6z9_C9CeTAYLzIA82LJ09ZNtLUdNQrES9K6Sq_xhGnM91x5Dz_Q-M-UMuTApDMjEcHHhFYVGBgjiWZ0nUIwh7U?key=oZ3gdgHh9pSWu4oJzdf49w)
    
- EVM can access and store data in 6 places
    

1.) Stack, 2.) Memory, 3.) Storage, 4.)Calldata, 5.)Code, 6.)Logs

- Calldata and Memory exist only temporarily, for the duration of the function call
    
- Calldata is temp variable but can’t be modified, memory can be
    
- Storage variable is permanent variable that can be modified
    
- Any variable declared outside of func inside of contract will be implicity converted to storage variable
    
- Only special types like string, struct and tuples we need to mention the data locations like memory or calldata
    
- Virtual Machine, message call: EVM can send msg to other accounts
    
- Message call is limited to 1024 levels
    
- Msg instruction is triggered by CALL instruction
    
- Args and return values are passed using memory
    