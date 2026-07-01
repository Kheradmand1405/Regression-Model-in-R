# مدل رگرسیون خطی ساده و چند متحوله در R

در این مثال از داده‌های واقعی موجود در بسته AER استفاده می‌شود. هدف، بررسی تأثیر تحصیلات بر عایدساعتی در مدل رگرسیون خطی ساده و سپس بررسی هم‌زمان اثر تحصیلات، تجربه کاری و سن بر دستمزد در مدل رگرسیون خطی چندگانه است.

---

## R Script

```r
## Install packages
install.packages(c("AER","ggplot2"))

## Load libraries
library(AER)
library(ggplot2)

## Load data
data("CPS1985")
```


# مدل رگرسیون خطی ساده

مدل رگرسیون خطی ساده رابطه میان یک متحول وابسته و یک متحول مستقل را به‌صورت زیر بیان می‌کند:

```math
Y_i=\beta_0+\beta_1X_i+\varepsilon_i
```
که در آن:

| نماد | توضیح |
|------|-------|
| $Y_i$ | مقدار متحول وابسته در مشاهده $i$ |
| $X_i$ | مقدار متحول مستقل در مشاهده $i$ |
| $\beta_0$ | عرض از مبدأ یا مقدار ثابت مدل |
| $\beta_1$ | ضریب رگرسیون(ضریب زاویه) که بیانگر تغییر متوسط متحول وابسته در اثر یک واحد تغییر در متحول مستقل است |
| $\varepsilon_i$ | جمله خطا که اثر سایر عوامل مؤثر بر متحول وابسته را که در مدل وارد نشده‌اند، نشان می‌دهد |
| $i=1,2,\ldots,n$ | شاخص مشاهده‌ها |

کد مدل رگرسیونی خطی ساده را در نرم‌افزار R می‌توان به‌صورت زیر نوشت:
```r
simple_model <- lm(wage ~ education, data = CPS1985)

summary(simple_model)
```
## خروجی نرم‌افزار R
```r

Call:
lm(formula = wage ~ education, data = CPS1985)

Residuals:
   Min     1Q Median     3Q    Max 
-7.911 -3.260 -0.760  2.240 34.740 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept) -0.74598    1.04545  -0.714    0.476    
education    0.75046    0.07873   9.532   <2e-16 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 4.754 on 532 degrees of freedom
Multiple R-squared:  0.1459,	Adjusted R-squared:  0.1443 
F-statistic: 90.85 on 1 and 532 DF,  p-value: < 2.2e-16
```

```math
Y_i=-0.74598+0.75046 education+\varepsilon_i
```
در این مدل، دستمزد ساعتی به‌عنوان متحول وابسته و سال‌های تحصیل به‌عنوان متحول مستقل در نظر گرفته شده است. خروجی تابع summary() ضرایب رگرسیون، مقدار آماره \(t\)، سطح معنی‌داری، ضریب تعیین \(R^2\) و آزمون \(F\) را گزارش می‌کند. اگر فرد یک سطح تحصیلی را با موفقیت به پایان می‌رساند، می‌توان نتیجه گرفت که 0.75046 واحدپولی، عاید ساعتی نیز به‌طور متوسط افزایش می‌یابد. مقدار \(R^2\) نیز نشان می‌دهد که چه درصدی از تغییرات دستمزد توسط متحول تحصیلات تبیین می‌شود.
```r
ggplot(CPS1985,
       aes(x = education,
           y = wage)) +
  geom_point() +
  geom_smooth(method = "lm") +
  labs(title = "Simple Linear Regression",
       x = "Years of Education",
       y = "Hourly Wage")
```
# گراف رگرسیون خطی ساده با استفاده از داده‌های CPS1985
![image alt](https://github.com/Kheradmand1405/Regression-Model-in-R/blob/90fbf756cbd258ef37405aba9bc98b61dbca6b8f/simple%20linear%20regression.png)
# رگرسیون چندگانه
در بسیاری از مسائل اقتصادی، متحول وابسته به‌طور هم‌زمان تحت تأثیر چندین متحول مستقل قرار می‌گیرد. در چنین شرایطی از مدل رگرسیون خطی چندگانه استفاده می‌شود که به‌صورت زیر نوشته می‌شود:

```math
Y_i=\beta_0+\beta_1X_{1i}+\beta_2X_{2i}+\cdots+\beta_kX_{ki}+\varepsilon_i
```
که در آن:

| نماد | توضیح |
|------|-------|
| $Y_i$ | متحول وابسته |
| $X_{1i},X_{2i},\ldots,X_{ki}$ | متحول‌های مستقل |
| $\beta_0$ | مقدار ثابت مدل |
| $\beta_1,\beta_2,\ldots,\beta_k$ | ضرایب رگرسیون(ضریب زاویه) که میزان اثر هر متحول مستقل بر متحول وابسته را نشان می‌دهند |
| $\varepsilon_i$ | جمله خطای تصادفی |
| $k$ | تعداد متحول‌های مستقل |
| $i=1,2,\ldots,n$ | شاخص مشاهده‌ها |
```r
multiple_model <- lm(
  wage ~ education + experience + age,
  data = CPS1985
)

summary(multiple_model)
```
# خروجی نرم‌افزار R
```r

Call:
lm(formula = wage ~ education + experience + age, data = CPS1985)

Residuals:
   Min     1Q Median     3Q    Max 
-8.351 -2.857 -0.599  1.994 36.336 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)
(Intercept) -4.76987    7.04271  -0.677    0.499
education    0.94833    1.15524   0.821    0.412
experience   0.12756    1.15571   0.110    0.912
age         -0.02241    1.15475  -0.019    0.985

Residual standard error: 4.604 on 530 degrees of freedom
Multiple R-squared:  0.202,	Adjusted R-squared:  0.1975 
F-statistic: 44.73 on 3 and 530 DF,  p-value: < 2.2e-16
```
گراف رگرسیون چندگانه در نرم‌افزار R به صورت زیر ترسیم می‌شود:
```r
ggplot(CPS1985,
       aes(x = education,
           y = wage,
           color = experience)) +
  geom_point() +
  geom_smooth(method = "lm",
              se = FALSE) +
  labs(title = "Multiple Linear Regression",
       x = "Years of Education",
       y = "Hourly Wage")
```

در این مدل، تحصیلات، تجربه کاری و سن به‌صورت هم‌زمان وارد مدل شده‌اند تا اثر هر متحول در حضور سایر متحول‌ها اندازه‌گیری شود. خروجی summary() علاوه بر ضرایب برآوردشده، آزمون معنی‌داری هر ضریب، مقدار \(R^2\)، مقدار \(R^2\) تعدیل‌شده و آزمون کلی \(F\) را نمایش می‌دهد. در صورتی که ضرایب متحول‌ها از نظر احصائیه‌ای معنی‌دار باشند، می‌توان نتیجه گرفت که هر یک از این عوامل، با ثابت نگه‌داشتن سایر عوامل، بر عاید ساعتی اثرگذار هستند. این مدل معمولاً نسبت به مدل رگرسیون خطی ساده قدرت تبیین بیشتری دارد؛ زیرا چندین عامل اقتصادی را به‌طور هم‌زمان در تحلیل وارد می‌کند
