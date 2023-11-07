## Quiz 2 - Probability, Statistics (ლექცია 4, ლექცია 5) - 7 ქულა

იხილეთ LoanStatus.csv მონაცემთა ფაილი, რომელშიც მოცემულია კლიენტთა ინფორმაცია შემდეგი სვეტებით:

Loan ID - კლიენტის ID

Gender - სქესი

Married - დაქორზინებული არის თუ არა

Dependents - ოჯახის წევრების რაოდენობა, რომლის ფინანსურ მხარდაჭერასაც უზრუნველყოფს კლიენტი

Education - არის თუ არა უმაღლესი განათლების მქონე

Self_Employed - არის თუ არა თვითდასაქმებული

ApplicantIncome - კლიენტის შემოსავალი (per month)

LoanAmount  - სესხის რაოდენობა (ათასებში)

Loan_Amount_Term - სესხის ვადა

Credit_History - როგორი საკრედიტო ისტორია აქვს კლიენტს (1-კარგი, 0-ცუდი)

Property_Area - საცხოვრებელი და სამუშაო ადგილი

Loan_Status - სესხის გაცემის სტატუსი

## დავალება 1: აღწერილობითი სტატისტიკა  (1 ქულა)
ა) გახსენით ფაილი და დაბეჭდეთ აღწერილობითი სტატისტიკის მონაცემები თქვენთვის საინტერესო ველებისთვის, როგორიცაა საშუალო, მედიანა, მოდა, სტდ. გადახრა, ა.შ. (მოახდინეთ სიტყვიერი ინტერპრეტაცია);

ბ) დაითვალეთ თითოეულ სვეტში ცარიელი მნიშვნელობების რაოდენობა შესაბამისი ფუნქციით
import pandas as pd
import warnings

from scipy.stats import norm
warnings.filterwarnings("ignore")
df = pd.read_csv("LoanStatus.csv")
columns_to_analyze = ["ApplicantIncome", "LoanAmount"]

for column in columns_to_analyze:
    column_min = df[column].min()
    column_max = df[column].max()
    column_mean = df[column].mean()
    column_median = df[column].median()

    print(f"Column: {column}")
    print(f"Minimum: {column_min}")
    print(f"Maximum: {column_max}")
    print(f"Mean: {column_mean}")
    print(f"Median: {column_median}")
    print("\n")

blank_count = df.isnull().sum()
print(blank_count)
## დავალება 2:  ალბათობა (1 ქულა)
ა) დაითვალეთ, რა არის სესხის აღების ალბათობა Loan_Status-ის მიხედვით.

ბ) რა არის სესხის აღების ალბათობა, მაშინ როცა კლიენტს კარგი საკრედიტო ისტორია აქვს.

კოდს დაურთეთ თქვენი კომენტარები მოკლედ
loan_approval_probability = df['Loan_Status'].value_counts(normalize=True)
print(loan_approval_probability)
good_credit_history_approval = df[df['Credit_History'] == 1]['Loan_Status'].value_counts(normalize=True)
print(good_credit_history_approval)
## დავალება 3: გრაფიკული წარმოდგენა (1 ქულა)
ა) seaborn.distplot ან seaborn.histplot() ფუნქციის გამოყენებით, ააგეთ რომელიმე სვეტისთვის შესაბამისი გრაფიკი. ლინკი: https://seaborn.pydata.org/generated/seaborn.distplot.html

ბ) matplotlib.pyplot.hist ფუნქციის გამოყენებით, ააგეთ რომელიმე სვეტისთვის შესაბამისი ჰისტოგრამი, რომელშიც bin-ების (ჰოსტოგრამაში ბლოკების) რაოდენობას თქვენ განსაზღვრავთ (მაგ. ჰისტოგრამაში 10 სვეტად წარმოადგინოთ მონაცემები.) იხ. დოკუმენტაცია შემდეგ ლინკზე: https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.hist.html 

Future warnings-ების გამოსართავად ჩაწერეთ შემდეგი ბრძანებები:
import warnings
warnings.filterwarnings("ignore")

კოდს დაურთეთ თქვენი კომენტარები მოკლედ
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
sns.histplot(data=df, x="ApplicantIncome", kde=True)

plt.xlabel("Income")
plt.ylabel("Frequency")
plt.title("Applicant Income")

plt.show()
plt.hist(df["ApplicantIncome"], bins=10, edgecolor='k', alpha=0.7)

plt.xlabel("Applicant Income")
plt.ylabel("Frequency")
plt.title("Histogram of Applicant Income (10 Bins)")
plt.show()
## დავალება 4:  კუმულაციური ალბათობა (Cumulative distribution function) - (1 ქულა)
დაითვალეთ შემდეგი კუმულაციური ალბათობა scipy.stats.norm.cdf() ფუნქციის გამოყენებით.
დაითვალეთ კლიენტების რამდენ პროცენტს აქვს შემოსავალი 2000 ევროზე ნაკლები ყოველთვიურად.
norm.cdf(x, mean_val, std_dev_val) ფუნქციის პირველი პარამეტრია სასაზღვრო მნიშვნელობა, მეორე- საშუალო, მესამე -სტდ. გადახრა.

#### მოახდინეთ შედეგების სიტყვიერი ინტერპრეტაცია
from scipy.stats import norm
mean_val = 6000
std_dev_val = 2000

x = 2000
cumulative_probability = norm.cdf(x, loc=mean_val, scale=std_dev_val)

percentage = cumulative_probability * 100

print(f"clients earning less than 2000  {percentage:.2f}%.")


## დავალება 5: სტატისტიკა (1 ქულა)

ააგეთ 2 boxplot დიაგრამა seaborn.boxplot() ფუნქციის გამოყენებით რომელიმე ველის მიმართ. 
sns.boxplot(x="ApplicantIncome", data=df)

# Set labels and title
plt.xlabel("Applicant Income")
plt.title("Box Plot of Applicant Income")

# Show the plot
plt.show()
sns.boxplot(x="LoanAmount", data=df)

plt.xlabel("Loan Amount")
plt.title("Box Plot of Loan Amount")

plt.show()
## დავალება 6: t-test (2 ქულა)
ა) გააკეთეთ t-test ანალიზი (One sample t-test)  რომელიმე სვეტის მიმართ და გამოიყენეთ  ttest_1samp ფუნქცია. 

ბ) გააკეთეთ t-test ანალიზი (Two sample t-test) რომელიმე სვეტის მიმართ და გამოიყენეთ  ttest_ind ფუნქცია. 

განსაზღვრეთ ნულოვანი და ალტერნატიული ჰიპოთეზა, გამოიყენეთ აღნიშნული ფუნქციები და მოახდინეთ შედეგების სიტყვიერი ინტერპრეტაცია.
from scipy.stats import ttest_1samp,ttest_ind
null_hypothesis_mean = 6000

t_statistic, p_value = ttest_1samp(df['ApplicantIncome'], null_hypothesis_mean)

alpha = 0.05

if p_value < alpha:
    print("Reject the null. income is different from 6000")
else:
    print("Fail to reject the null. income is not significantly different from 6000 euros.")

approved = df[df['Loan_Status'] == "Y"]['ApplicantIncome']
not_approved = df[df['Loan_Status'] == "N"]['ApplicantIncome']

t_statistic, p_value = ttest_ind(approved, not_approved, equal_var=False)

alpha = 0.05

if p_value < alpha:
    print("Reject the null hypothesis. The mean income of received loans is different from;")
else:
    print("There is no significant difference in income between the two groups.")

