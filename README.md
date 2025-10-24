[Orders_with_issues.py](https://github.com/user-attachments/files/23131906/Orders_with_issues.py)
import numpy as np
import pandas as pd

#_-------load data and inspecting row data for the Wrangling 

# df= pd.read_csv("D:/Python/New Course/Orders_with_issues.csv")
df= pd.read_csv("D:\\Python\\New Course\\Orders_with_issues.csv")

# print(df.head()) 

# print(df.tail())  

# print(df.info())

# print(df.describe())

# print(df.isna().sum())

# print(df['ShippingCompany'].value_counts())  ---- count unique values

# _____________________________________________________________________________________________________________________________

#---------- Data Cleaning --------

###convert str date to acual data & if contain undefines values retrive NAT

df['OrderDate']=pd.to_datetime(df['OrderDate'],errors='coerce')
df['ShippedDate']=pd.to_datetime(df['ShippedDate'],errors='coerce')


# ###convert str numbers to acual decimal numbers and ignore negative (clean shipping cost)
df['ShippingCost']=pd.to_numeric(df['ShippingCost'],errors='coerce')

df.loc[df['ShippingCost'] <0 ,'ShippingCost']= np.nan # --- if it has negative values replace with NAN

# ___________________________________________________________________________________________________________________________________

###                -------   Handling Nulls       -------     #####

df['OrderID']=df['OrderID'].ffill()
# -- if orderid null then fill with the last orderid

df['CustomerID']=df['CustomerID'].fillna("UnKnown")
# # -- if customerid is null fill with "unkowon"

df['ShipCity']=df['ShipCity'].fillna("UnSpecified")

# -- if shipcity is null fill with "unspecified"

# __________________________________________________________________________________________________________________________________

###          -------  Standardize names   -------     #####

##### delete any spaces or tabs in two sides (strip) & make the first letter of each word capital (title)

df['ShipCountry']=df['ShipCountry'].str.strip().str.title()
df['ShipCity']=df['ShipCity'].str.strip().str.title()

df['ShippingCompany']=df['ShippingCompany'].str.strip()


###### Fix specific Word 

# if found any Shippingcompany contain 'Kiwilytics' use 'Kiwilytics Goods Shipping LLC.'

df.loc[df['ShippingCompany'].str.contains("Kiwilytics",na=False),'ShippingCompany']="Kiwilytics Goods Shipping LLC."

# _____________________________________________________________________________________________________________________________________

#### ----- Feature Engineering

# Days between shipping and order date

df['DeliveryDate'] = df['ShippedDate']-df['OrderDate']

# --- Flags that show the late orders ( delivery days = 30 )

def get_status(x):
    if pd.isna(x):
        return "Unknown"
    elif x >30:
        return "Late"
    else:
        return "Ontime"

# get_status(31)
# print(df)


# ------------------------------------------------------------------

#Avg of Shipping Cost
# Avg_shippingcost_bycompany=df.groupby("ShippingCompany")['ShippingCost'].mean()
# print("The Average Cost of each company is  : " , Avg_shippingcost_bycompany)


# _________________________________________________________________________________________________________

###------ Export Clean file

# df.to_csv("Cleaned_Order_Final.csv" , index=False)

# --- Orders by Country

print(df["ShipCountry"].value_counts())

# --- Order By City 
print(df["ShipCity"].value_counts())

# --- Top 3 Shipping Companies

print(" The Top 3 Shipping Companies are  : \n " , df["ShippingCompany"].value_counts().head(3) )
# print(df)










































































































# # df=pn.read_csv("D:\System Data\Downloads\Orders_with_issues.csv")

# # print(df.head())
# # print(df.tail())
# # print(df.isna().sum())
# # print(df.isna())
# # print(df.describe())
# # print(df[["ShipCountry","OrderID"]])

# # print(df["ShipCountry"].upper())
# # print(df.info())

# # grouped_company=df.groupby("ShippingCompany").value_counts("OrderID")
# # print(grouped_company)


# ##############       Count   #####
# # print(df["ShippingCompany"].value_counts())

# ###################### Convert into Date Time ###########################

# # df["OrderDate"] = pn.to_datetime(df["OrderDate"], errors="coerce")
# # df["ShippedDate"] = pn.to_datetime(df["ShippedDate"], errors="coerce")

# # df["ShippingCost"] = pn.to_numeric(df["ShippingCost"] , errors= "coerce")

# # df["ShippingCost"] = df["ShippingCost"] < 0
# # print(df)


# # --------------########     Matplotlib------------------
# # Sample data
# from matplotlib import pyplot as plt
# import numpy as np


# x = [1, 2, 3, 4, 5]
# y = [2, 4, 6, 8, 10]
# # plt.plot(x,y)
# plt.xlabel("x_axis")
# plt.ylabel("y_axis")
# plt.title("Graph one")
# plt.show()
# # plt.plot(x, y, marker='o')
# # plt.title("Test Plot")
# # plt.xlabel("X-axis")
# # plt.ylabel("Y-axis")
# # plt.show()

