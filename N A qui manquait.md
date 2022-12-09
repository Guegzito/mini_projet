# Data et subset

```
rm(list = ls())

library(ggplot2)

#charger les jeux de données manuellement

library(readr)
A9066502 <- read_csv2("A9066502.CSV")
View(A9066502)

####################jeu de données A9066502###################

#charger le jeu de données 
jour_7 <- A9066502

#passer de kelvin en celsius : -273.15 â REGLER
jour_7$Value2 <- (jour_7$Value2) - 273.15

#passage des données temporelle en posixct 
jour_7$`Date/Time` <- as.POSIXct(jour_7$`Date/Time`, format = "%d.%m.%Y %H:%M:%S")

#subset des données sur notre manip
which(jour_7$`Date/Time` == "2022-12-06 19:34:19")
which(jour_7$`Date/Time` == "2022-12-06 20:38:01")
respiroJ7 <- jour_7[ 9196:9439 , ]

#Subset des données par sondes 
sonde_16142185 <- subset(respiroJ7, subset = respiroJ7$Additional == 'Sal = 35.0   SC-FDO 925   16142185   t90 = 30 s')

#subset des données par échantillon 
#subset par échantillon ; A pour avant et P pour après
```

# Sonde 16142185
## Echantillon N A

```
which(sonde_16142185$`Date/Time` == "2022-12-06 19:34:19")
which(sonde_16142185$`Date/Time` == "2022-12-06 20:04:19")
A_N <- sonde_16142185[ 1:61 , ]

#regression 
ggplot(data = A_N) + 
  geom_point(mapping = aes(x = `Date/Time`, y = Value)) +
  geom_smooth(mapping = aes(x = `Date/Time`, y = Value)) +
  xlab("Time") + ylab("mg O2.L^{-1}") 

ggplot(data = A_N) + 
  geom_point(mapping = aes(x = `Date/Time`, y = Value2)) +
  xlab("Time") + ylab("Température") 

#reprendre ici !!!!! 
par(mfrow = c(1,1))
plot(A_N$`Date/Time`, A_N$Value)
lm1 <- lm(A_N$Value ~ A_N$`Date/Time`, data = A_N)
lm1

par(mfrow=c(2,2))
plot(lm1)
par(mfrow = c(1,2))
coef(lm1)

plot(Value ~ `Date/Time`, data=A_N) # left plot
abline(lm1) # line described by model parameters
hist(residuals(lm1))
shapiro.test(residuals(lm1))
skewness(residuals(lm1))

R_A_N <- -1.456549e-04 #mg.O2.L-1 #par min?
```
