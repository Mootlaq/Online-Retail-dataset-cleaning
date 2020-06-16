The dataset contains transactions of an online store that sell gift-ware between 01/12/2009 and 09/12/2011. Customers are wholesalers. The online store is UK-based.

#### Concate two sheets: 
```
df = pd.read_excel('online_retail_II.xlsx', sheet_name=0)
df1 = pd.read_excel('online_retail_II.xlsx', sheet_name=1)
df = pd.concat([df, df1], ignore_index=True)
```


#### Inestigating Missing Values
- Description has 4382 missing values but that's ok as long as we have the stock code of the product. 
- customer ID also has missing values and that might be because it's a guest purchase. 
- After invistigating the records with missing customer ID values, the records seems to make sense. 
- No need to drop the records. 

```
na_df = df.isna()
df_mask = na_df.any(axis=1)
df.loc[df_mask,:]
```

#### Change date type
```
df['InvoiceDate'] = pd.to_datetime(df['InvoiceDate'])
```

- What is the average amount of items bought per transaction?
	- Oops! A negative number were found! It doesn't make sense. 
	- 22950 records with negative values!! dig more. 
		- I found -1000! is (1000) even normal? check. 
- Some records includes items with 0GBP price which seems a bit strange, when we investigate these records we can find that most of the dersciptions say smashed, damaged, discoloured, thrown away or lost. Another thing the records show is that these trasnsactions are not associated with customers, and the quantity column is usually a negative quantity. To me these records don't indicate sales so let's drop them.
- Another strange thing we found in the dataset is that there are about 19500 records with a negative quantity which doesn't make sense. Investigating the matter further shows that the invoice number in these transactions include the latter C which indicates cancellation. So we need to drop these records as they are considered sales. But if these orders were cancelled, we need to check the initial transactions. if they are in the dataset, we need to drop them too. 
	- *UPDATE*: When investigating the dataset, a lot of cancelled orders didn't have an intital order. Also, If there was actually an initial order it was made after the cancelled order which indicates that it's not actually an 'initial order.' Yet, It probably means that the customer made an order, it was cancelled due to a certain problem (wrong payment method for example), The order was fixed and made! 
	- So, all what we have to do now is dropping the cancelled orders. 
	- This process took about 3 hours. So, I'll take this chance to emphasize the importance of patience and taking time exploting the data and innvestigating what's there and what's not. You're gonna use this data to train your model so If you want your model to make good prediction, you need to feed it good accurate data.


