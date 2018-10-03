+++
author = "Larissa"
categories = ["R"]
tags = ["simulation", "first-differences", "machine-learning", "neural-nets", "regression", "spring2018", "code"]
date = "2018-06-15"
description = "Comparing different ML Approaches on Data on Voluntary Action - Advanced Quantitative Methods - Spring Semester 2018"
featured = ""
featuredpath = "date"
linktitle = ""
title = "Neural Net Paper - R Code Appendix"
type = "post"

+++

R Code
============

The code used for this paper was written with the help of the tutorial
held by Marcel Neunhoeffer (provided by the chair of Quantitative
Methods in the Social Sciences) to support the lecture Advanced
Quantitative Methods, as well as with the provided R code by (Lantz
2015; Cook 2017; Dr. Wiley 2016).

```R
    ##################################################################

    # Local R-File
    # Final Submission for AQM
    # Larissa Haas

    ##################################################################

    # Note: The plots and long running code snippets are
    #       commented out. Please consider if it is really 
    #       necessary to run them, as this might take some while.

    # Eventually, I do a lot of plotting in the code, but I can't 
    # show everything in my paper. I didn't want to delete it, either.
    # I hope the code is nevertheless interesting.

    ##################################################################
    # Setting up the Working Environment
    ##################################################################
    library(neuralnet)
    library(devtools)
    library(stargazer)
    library(RCurl)
    library(jsonlite)
    library(caret)
    library(ggplot2)
    library(e1071)
    library(h2o)
    library(statmod)
    library(MASS)
    library(corrplot)
    library(data.table)
    library(ROSE)
    library(reshape2)
    library(ggpubr)

    F1 <- function(table){
        tp <- table[2,2]
        tn <- table[1,1]
        fp <- table[2,1]
        fn <- table[1,2]
        precision <- tp / (tp + fp)
        recall <- tp / (tp + fn)
        result <- (2 * precision * recall) / (precision + recall)
        return(result)
    }

    ACC <- function(table){
        tp <- table[2,2]
        tn <- table[1,1]
        fp <- table[2,1]
        fn <- table[1,2]
        acc <- (tn + tp) / (tn + tp + fn + fp)
        return(acc)
    }

    ll_logit <- function(theta, y, X) {
        beta <- theta[1:ncol(X)]
        mu <- X %*% beta
        p <- 1/(1 + exp(-mu))
        ll <- y * log(p) + (1 - y) * log(1 - p)
        ll <- sum(ll)
        return(ll)
    }

    ##################################################################
    # Loading and Pre-Processing the Data
    ##################################################################

    ess1 <- read.table("C:/Users/laris/Desktop/MMDS/Semester3-FSS2018/Advanced-Quantitative-Methods/paper/Submission/ess1.csv", header=TRUE, sep=",") 

    ess6 <- read.table("C:/Users/laris/Desktop/MMDS/Semester3-FSS2018/Advanced-Quantitative-Methods/paper/Submission/ess6.csv", header=TRUE, sep=",") 

    ess1$round1 = 1
    ess6$round1 = 0

    data <- rbind(ess1, ess6)

    dim(data)

    data.complete <- data

    for(i in c(4,5,6,10,11,12,13,14,15,16,17,18,19,20,21,22,23)){
        data.complete[is.na(data.complete[,i]), i] <- mean(data.complete[,i], na.rm = TRUE)
    }

    data.complete[is.na(data.complete[,3]), 3] <- 1
    data.complete[is.na(data.complete[,7]), 7] <- 1
    data.complete[is.na(data.complete[,8]), 8] <- 0
    data.complete[is.na(data.complete[,9]), 9] <- 0

    data.scaled <- cbind(data.complete[c(1, 3, 7, 8, 9, 25)], scale(data.complete[-c(1, 2, 3, 7, 8, 9, 24, 25)]))

    # This part can be used to write and to load 
    # the already pre-processed data

    #write.csv(data.complete, file = "ess-complete.csv")
    #write.csv(data.scaled, file = "ess-scaled.csv")

    #data.scaled <- read.table("ess-scaled.csv", header=TRUE, sep=",")
    #data.scaled <- data.scaled[,-1]

    # Splitting in Train and Test Data

    smp_size <- floor(0.9 * nrow(data.scaled))
    set.seed(123)
    train_ind <- sample(seq_len(nrow(data.scaled)), size = smp_size)
    train <- data.scaled[train_ind,]
    test <- data.scaled[-train_ind,]

    # Balancing the Data

    train.bal <- ovun.sample(volact ~ ., data = train, method = "over", N = 114264)$data

    # Defining X and y for later use

    train.X <- train[, -1]
    train.y <- train[, 1]
    train.y <- factor(train.y, levels = 0:1)
    train.X.bal <- train[, -1]
    train.y.bal <- train[, 1]
    train.y.bal <- factor(train.y, levels = 0:1)
    test.X <- test[, -1]
    test.y <- test[, 1]
    test.y <- factor(test.y, levels = 0:1)

    ##################################################################
    # First Step into Neural Nets
    ##################################################################

    # In this section I want to replicate the code we worked on 
    # in the tutorial. While doing this, necessary functions are
    # defined. But most of the code is just "getting warm" wit NNs.

    ##################################################################

    col <- test$volact
    col[test$volact == 1] <- adjustcolor("orange", alpha = 0.3)
    col[test$volact == 0] <- adjustcolor("blue", alpha = 0.3)

    #plot(test$wkhtot, test$social_trust, col = col, pch = 19,
    #     main = "True Relationship \n (scaled values)",
    #     bty = "n", las = 1,
    #     xlab = "Total Work Hours (per Week)", ylab = "Social Trust")

    y <- test$volact
    X <- cbind(1, test$wkhtot, test$social_trust)

    startvals <- c(0, 0, 0)

    res <- optim(par = startvals,fn = ll_logit, y = y, X = X,
                  control = list(fnscale = -1),
                  method = "BFGS"
                  ) 

    mu <- X %*% res$par

    p <- 1/(1 + exp(-mu))

    y_hat <- rbinom(nrow(p), 1, p)
    col <- y_hat
    col[y_hat == 1] <- adjustcolor("orange", alpha = 0.3)
    col[y_hat == 0] <- adjustcolor("blue", alpha = 0.3)

    #plot(test$wkhtot, test$social_trust, col = col, pch = 19,
    #      main = "True Relationship",
    #     bty = "n", las = 1,
    #     xlab = "x1", ylab = "x2")

    logit_pred <- table(y_hat, test$volact)
    #logit_pred

    ll_simple_nn <- function(theta, y, X){
      
      gamma <- theta[1:4]
      beta_neuron1 <- theta[5:7]
      beta_neuron2 <- theta[8:10]
      beta_neuron3 <- theta[11:13]
      
      mu_neuron1 <- X %*% beta_neuron1
      mu_neuron2 <- X %*% beta_neuron2
      mu_neuron3 <- X %*% beta_neuron3
      
      logitResponse <- function(mu) 1 / (1+exp(-mu))
      
      p_neuron1 <- logitResponse(mu_neuron1)
      p_neuron2 <- logitResponse(mu_neuron2)
      p_neuron3 <- logitResponse(mu_neuron3)
      
      Z <- cbind(1, p_neuron1, p_neuron2, p_neuron3)
      
      mu <- Z %*% gamma
      
      p <- logitResponse(mu)
      
      ll <- y * log(p) + (1 - y) * log(1 - p)
      
      ll <- sum(ll)
      return(ll)
    }

    # initial values
    startvals <- rnorm(13)

    ll_simple_nn(startvals, y, X)

    # optimize
    resNN <- optim(par = startvals, fn = ll_simple_nn, y = y, X = X,
                  control = list(fnscale = -1),
                  hessian = F,
                  method = "BFGS"
                  ) 

    #resNN$par

    gammaEst <- resNN$par[1:4]
    beta_neuron1Est <- resNN$par[5:7]
    beta_neuron2Est <- resNN$par[8:10]
    beta_neuron3Est <- resNN$par[11:13]

    mu_neuron1Est <- X %*% beta_neuron1Est
    mu_neuron2Est <- X %*% beta_neuron2Est
    mu_neuron3Est <- X %*% beta_neuron3Est

    logitResponse <- function(mu) 1/(1+exp(-mu))

    p_neuron1Est <- logitResponse(mu_neuron1Est)
    p_neuron2Est <- logitResponse(mu_neuron2Est)
    p_neuron3Est <- logitResponse(mu_neuron3Est)

    Z <- cbind(1, p_neuron1Est, p_neuron2Est, p_neuron3Est )

    mu <- Z %*% gammaEst

    p <- logitResponse(mu)


    y_hat <- rbinom(nrow(p),1,p)
    col <- y_hat

    col[y_hat == 1] <- adjustcolor("orange", alpha = 0.3)
    col[y_hat == 0] <- adjustcolor("blue", alpha = 0.3)

    #plot(X[, 2], X[, 3], col = col, pch = 19,
    #     main = "Predicted Values from a Neural Net",
    #     bty = "n", las = 1,
    #     xlab = "x1", ylab = "x2")

    nn_pred <- table(y_hat, test$volact)
    #nn_pred

    m <- neuralnet(volact ~ wkhtot + social_trust, 
                  train, hidden = 1)

    p <- compute(m, test[,c(11, 14)])

    predictions <- p$net.result

    result_test <- rbinom(nrow(predictions),1,predictions)
    result3 <- ifelse(predictions >= 0.3, 1, 0)
    result2 <- ifelse(predictions >= 0.2, 1, 0)
    result4 <- ifelse(predictions >= 0.4, 1, 0)

    print(cor(result_test, test$volact))

    prenn_pred <- table(result_test, test$volact)
    #prenn_pred

    #png("basic_nn.png", width = 800, height = 600)
    #plot.nnet(m)
    #dev.off()

    # Comparison of the PRE-defined NN, our custom NN and the 
    # logistic regression (based on two input variables).

    F1(nn_pred)
    F1(logit_pred)
    F1(prenn_pred)
    F1(table(result2, test$volact))
    F1(table(result3, test$volact))
    F1(table(result4, test$volact))

    ##################################################################
    # Logistic Regression: Calculated on the "Basic Model" Data
    ##################################################################

    y <- train$volact
    X <- as.matrix(cbind(1, train[,-1]))

    startvals <- c(rep(0, 23))
    res <- optim(par = startvals,fn = ll_logit, y = y, X = X,
                  control = list(fnscale = -1),
                  method = "BFGS"
                  ) 

    mu <- as.matrix(cbind(1, test[,-1])) %*% res$par
    p <- 1/(1 + exp(-mu))

    # The following code snippet will be repeated more often, because
    # I can have a look at different thresholds for evaluating the 
    # logistic regression results. The output will always be:
    # 1. MAX Accuracy
    # 2. MAX F1 measure

    y_hat1 <- ifelse(p > 0.1, 1, 0)
    y_hat2 <- ifelse(p > 0.2, 1, 0)
    y_hat3 <- ifelse(p > 0.3, 1, 0)
    y_hat4 <- ifelse(p > 0.4, 1, 0)
    y_hat5 <- ifelse(p > 0.5, 1, 0)
    y_hat6 <- ifelse(p > 0.6, 1, 0)
    y_hat7 <- ifelse(p > 0.7, 1, 0)
    y_hat8 <- ifelse(p > 0.8, 1, 0)
    y_hat9 <- ifelse(p > 0.9, 1, 0)

    threshold <- cbind(seq(0.1, 0.8, length.out = 8),
        
    c(ACC(table(y_hat1, test$volact)),
    ACC(table(y_hat2, test$volact)),
    ACC(table(y_hat3, test$volact)),
    ACC(table(y_hat4, test$volact)),
    ACC(table(y_hat5, test$volact)),
    ACC(table(y_hat6, test$volact)),
    ACC(table(y_hat7, test$volact)),
    ACC(table(y_hat8, test$volact))),
    #ACC(table(y_hat9, test$volact))),

    c(F1(table(y_hat1, test$volact)),
    F1(table(y_hat2, test$volact)),
    F1(table(y_hat3, test$volact)),
    F1(table(y_hat4, test$volact)),
    F1(table(y_hat5, test$volact)),
    F1(table(y_hat6, test$volact)),
    F1(table(y_hat7, test$volact)),
    F1(table(y_hat8, test$volact))))
    #F1(table(y_hat9, test$volact))))

    print("MAX Accuracy for BASIC Logistic Regression")
    max(threshold[,2])
    print("MAX F1 for BASIC Logistic Regression")
    max(threshold[,3])

    # The following plot shows the distribution of Accuracy and
    # F1 for the different threshold levels. A threshold with both
    # values high would be the best!

    #acc <- ggplot(data.frame(threshold),aes(threshold[,1],threshold[,2]))+geom_line(aes(color="Accuracy"))+
    #    labs(color=" ") + 
    #    ylab("accuracy value") + xlab("threshold")
    #f1 <- ggplot(data.frame(threshold),aes(threshold[,1],threshold[,4]))+geom_line(aes(color="F1"))+
    #    labs(color=" ") + 
    #    ylab("f1 value") + xlab("threshold")
    #png("evaluation_log_threshold_unbal.png") 
    #myplot <- ggarrange(acc, f1 + rremove("x.text"), 
    #          labels = c("ACC", "F1"),
    #          ncol = 2, nrow = 1)
    #print(myplot)
    #dev.off()
    #print(myplot)

    ##################################################################
    # Logistic Regression: Calculated on Balanced Data
    ##################################################################

    y.bal <- train.bal$volact
    X.bal <- as.matrix(cbind(1, train.bal[,-1]))

    startvals <- c(rep(0, 23))
    res <- optim(par = startvals,fn = ll_logit, y = y.bal, X = X.bal,
                  control = list(fnscale = -1), hessian = TRUE,
                  method = "BFGS"
                  ) 

    mu <- as.matrix(cbind(1, test[,-1])) %*% res$par

    p <- 1/(1 + exp(-mu))

    y_hat1 <- ifelse(p > 0.1, 1, 0)
    y_hat2 <- ifelse(p > 0.2, 1, 0)
    y_hat3 <- ifelse(p > 0.3, 1, 0)
    y_hat4 <- ifelse(p > 0.4, 1, 0)
    y_hat5 <- ifelse(p > 0.5, 1, 0)
    y_hat6 <- ifelse(p > 0.6, 1, 0)
    y_hat7 <- ifelse(p > 0.7, 1, 0)
    y_hat8 <- ifelse(p > 0.8, 1, 0)
    y_hat9 <- ifelse(p > 0.9, 1, 0)

    threshold <- cbind(seq(0.1, 0.9, length.out = 9),
        
    c(ACC(table(y_hat1, test$volact)),
    ACC(table(y_hat2, test$volact)),
    ACC(table(y_hat3, test$volact)),
    ACC(table(y_hat4, test$volact)),
    ACC(table(y_hat5, test$volact)),
    ACC(table(y_hat6, test$volact)),
    ACC(table(y_hat7, test$volact)),
    ACC(table(y_hat8, test$volact)),
    ACC(table(y_hat9, test$volact))),

    c(F1(table(y_hat1, test$volact)),
    F1(table(y_hat2, test$volact)),
    F1(table(y_hat3, test$volact)),
    F1(table(y_hat4, test$volact)),
    F1(table(y_hat5, test$volact)),
    F1(table(y_hat6, test$volact)),
    F1(table(y_hat7, test$volact)),
    F1(table(y_hat8, test$volact)),
    F1(table(y_hat9, test$volact))))

    print("MAX Accuracy for Balanced Logistic Regression")
    max(threshold[,2])
    print("MAX F1 for Balanced Logistic Regression")
    max(threshold[,3])

    #acc <- ggplot(data.frame(threshold),aes(threshold[,1],threshold[,2]))+
    #    geom_line(aes(color="Accuracy"))+
    #    labs(color=" ") + 
    #    ylab("accuracy value") + xlab("threshold")
    #f1 <- ggplot(data.frame(threshold),aes(threshold[,1],threshold[,4]))+
    #    geom_line(aes(color="F1"))+
    #    labs(color=" ") + 
    #    ylab("f1 value") + xlab("threshold")
    #png("evaluation_log_threshold_bal.png") 
    #myplot <- ggarrange(acc, f1 + rremove("x.text"), 
    #          labels = c("ACC", "F1"),
    #          ncol = 2, nrow = 1)
    #print(myplot)
    #dev.off()
    #print(myplot)

    ##################################################################
    # Logistic Regression: Tested with Pre-Defined Function
    ##################################################################

    ols_model <- glm(y ~ X, family=binomial(link='logit'))

    #summary(ols_model)

    ##################################################################
    # Logistic Regression: Preparing Plot for Marginal Effects later
    ##################################################################

    range <- seq(-4, 2, length.out = 20)
    sel <- 7
    plot.data <- matrix(NA, nrow = length(range), ncol = ncol(test[,-1]))
    for(i in 1:ncol(test[,-1])){
        plot.data[,i] <- mean(test[,i])
    }
    for(i in 1:length(range)){
        plot.data[i, sel] <- range[i]
    }

    mu <- as.matrix(cbind(1, plot.data)) %*% res$par
    p <- 1/(1 + exp(-mu))

    age.volact <- data.frame(cbind(range, p))

    ##################################################################
    # Neural Net: Plotting Nets with Neuralnet-Library
    ##################################################################

    #m <- neuralnet(volact ~ sclmeet, 
    #              train, hidden = 1)

    #p <- compute(m, test[,13])

    #predictions <- p$net.result

    #print(cor(predictions, test$volact))

    #result <- ifelse(predictions >= 0.3, 1, 0)
    #ACC(table(result, test$volact))
    #F1(table(result, test$volact))

    #plot.nnet(m)

    #m2 <- neuralnet(volact ~ sclmeet + social_trust + tolerance + 
    #               self_realisation + solidarity, 
    #               train, hidden = 2)

    #p2 <- compute(m2, test[,c(13, 14, 15, 16, 17)])

    #predictions2 <- p2$net.result

    #print(cor(predictions2, test$volact))

    #result3 <- ifelse(predictions2 >= 0.3, 1, 0)
    #result2 <- ifelse(predictions2 >= 0.2, 1, 0)
    #result4 <- ifelse(predictions2 >= 0.4, 1, 0)

    #ACC(table(result4, test$volact))
    #F1(table(result4, test$volact))

    #png("more-complex_nn.png", width = 800, height = 600)
    #plot.nnet(m2)
    #dev.off()

    #m3 <- neuralnet(volact ~ round1 + female + yrbrn + eduyrs + domicil + married + 
    #               children + houseperson + wkhtot + church + sclmeet + social_trust + 
    #               tolerance + self_realisation + solidarity + tvpol + tvtot + 
    #               political_interest + trust_exe + trust_leg + trstep + stfdem, 
    #               train, hidden = 1)

    #p3 <- compute(m3, test[,-c(1, 2, 25)])

    #predictions3 <- p3$net.result

    #print(cor(predictions3, test$volact))

    #library(nnet)
    #png("total-input_nn.png", width = 800, height = 600)
    #plot.nnet(m3)
    #dev.off()

    ##################################################################
    # Neural Net: Models for Plotting Tuning Differences (Part 1)
    ##################################################################

    # For this plot, I am tuning the decay rate, which is an
    # additional weight parameter influencing the signals between
    # the neurons.

    #aus: R deep learning essentials
    #m20.0 <- train(x = train.X, y = train.y,
    #           method = "nnet", 
    #           tuneGrid = expand.grid(
    #           .size = c(20),
    #           .decay = 0),
    #           trControl= trainControl(method = "none"),
    #           MaxNWts = 1000,
    #           maxit = 100)
    #m20.1 <- train(x = train.X, y = train.y,
    #           method = "nnet", 
    #           tuneGrid = expand.grid(
    #           .size = c(20),
    #           .decay = 0.1),
    #           trControl= trainControl(method = "none"),
    #           MaxNWts = 1000,
    #           maxit = 100)
    #m20.2 <- train(x = train.X, y = train.y,
    #           method = "nnet", 
    #           tuneGrid = expand.grid(
    #           .size = c(20),
    #           .decay = 0.2),
    #           trControl= trainControl(method = "none"),
    #           MaxNWts = 1000,
    #           maxit = 100)
    #m20.3 <- train(x = train.X, y = train.y,
    #           method = "nnet", 
    #           tuneGrid = expand.grid(
    #           .size = c(20),
    #           .decay = 0.3),
    #           trControl= trainControl(method = "none"),
    #           MaxNWts = 1000,
    #           maxit = 100)
    #m20.4 <- train(x = train.X, y = train.y,
    #           method = "nnet", 
    #           tuneGrid = expand.grid(
    #           .size = c(20),
    #           .decay = 0.4),
    #           trControl= trainControl(method = "none"),
    #           MaxNWts = 1000,
    #           maxit = 100)
    #m20.5 <- train(x = train.X, y = train.y,
    #           method = "nnet", 
    #           tuneGrid = expand.grid(
    #           .size = c(20),
    #           .decay = 0.5),
    #           trControl= trainControl(method = "none"),
    #           MaxNWts = 1000,
    #           maxit = 100)
    #m20.6 <- train(x = train.X, y = train.y,
    #           method = "nnet", 
    #           tuneGrid = expand.grid(
    #           .size = c(20),
    #           .decay = 0.6),
    #           trControl= trainControl(method = "none"),
    #           MaxNWts = 1000,
    #           maxit = 100)
    #m20.7 <- train(x = train.X, y = train.y,
    #           method = "nnet", 
    #           tuneGrid = expand.grid(
    #           .size = c(20),
    #           .decay = 0.7),
    #           trControl= trainControl(method = "none"),
    #           MaxNWts = 1000,
    #           maxit = 100)
    #m20.8 <- train(x = train.X, y = train.y,
    #           method = "nnet", 
    #           tuneGrid = expand.grid(
    #           .size = c(20),
    #           .decay = 0.8),
    #           trControl= trainControl(method = "none"),
    #           MaxNWts = 1000,
    #           maxit = 100)
    #m20.9 <- train(x = train.X, y = train.y,
    #           method = "nnet", 
    #           tuneGrid = expand.grid(
    #           .size = c(20),
    #           .decay = 0.9),
    #           trControl= trainControl(method = "none"),
    #           MaxNWts = 1000,
    #           maxit = 100)
    #m20.10 <- train(x = train.X, y = train.y,
    #           method = "nnet", 
    #           tuneGrid = expand.grid(
    #           .size = c(20),
    #           .decay = 1),
    #           trControl= trainControl(method = "none"),
    #           MaxNWts = 1000,
    #           maxit = 100)

    #yhat20.0 <- predict(m20.0)
    #yhat20.1 <- predict(m20.1)
    #yhat20.2 <- predict(m20.2)
    #yhat20.3 <- predict(m20.3)
    #yhat20.4 <- predict(m20.4)
    #yhat20.5 <- predict(m20.5)
    #yhat20.6 <- predict(m20.6)
    #yhat20.7 <- predict(m20.7)
    #yhat20.8 <- predict(m20.8)
    #yhat20.9 <- predict(m20.9)
    #yhat20.10 <- predict(m20.10)
    #yhat_unseen20.0 <- predict(m20.0, as.matrix(test.X))
    #yhat_unseen20.1 <- predict(m20.1, as.matrix(test.X))
    #yhat_unseen20.2 <- predict(m20.2, as.matrix(test.X))
    #yhat_unseen20.3 <- predict(m20.3, as.matrix(test.X))
    #yhat_unseen20.4 <- predict(m20.4, as.matrix(test.X))
    #yhat_unseen20.5 <- predict(m20.5, as.matrix(test.X))
    #yhat_unseen20.6 <- predict(m20.6, as.matrix(test.X))
    #yhat_unseen20.7 <- predict(m20.7, as.matrix(test.X))
    #yhat_unseen20.8 <- predict(m20.8, as.matrix(test.X))
    #yhat_unseen20.9 <- predict(m20.9, as.matrix(test.X))
    #yhat_unseen20.10 <- predict(m20.10, as.matrix(test.X))

    #measures <- c("AccuracyNull", "Accuracy", "AccuracyLower", "AccuracyUpper")

    #n20.0.insample <- caret::confusionMatrix(xtabs(~train.y + yhat20.0))
    #n20.1.insample <- caret::confusionMatrix(xtabs(~train.y + yhat20.1))
    #n20.2.insample <- caret::confusionMatrix(xtabs(~train.y + yhat20.2))
    #n20.3.insample <- caret::confusionMatrix(xtabs(~train.y + yhat20.3))
    #n20.4.insample <- caret::confusionMatrix(xtabs(~train.y + yhat20.4))
    #n20.5.insample <- caret::confusionMatrix(xtabs(~train.y + yhat20.5))
    #n20.6.insample <- caret::confusionMatrix(xtabs(~train.y + yhat20.6))
    #n20.7.insample <- caret::confusionMatrix(xtabs(~train.y + yhat20.7))
    #n20.8.insample <- caret::confusionMatrix(xtabs(~train.y + yhat20.8))
    #n20.9.insample <- caret::confusionMatrix(xtabs(~train.y + yhat20.9))
    #n20.10.insample <- caret::confusionMatrix(xtabs(~train.y + yhat20.10))
    #n20.0.outsample <- caret::confusionMatrix(xtabs(~test.y + yhat_unseen20.0))
    #n20.1.outsample <- caret::confusionMatrix(xtabs(~test.y + yhat_unseen20.1))
    #n20.2.outsample <- caret::confusionMatrix(xtabs(~test.y + yhat_unseen20.2))
    #n20.3.outsample <- caret::confusionMatrix(xtabs(~test.y + yhat_unseen20.3))
    #n20.4.outsample <- caret::confusionMatrix(xtabs(~test.y + yhat_unseen20.4))
    #n20.5.outsample <- caret::confusionMatrix(xtabs(~test.y + yhat_unseen20.5))
    #n20.6.outsample <- caret::confusionMatrix(xtabs(~test.y + yhat_unseen20.6))
    #n20.7.outsample <- caret::confusionMatrix(xtabs(~test.y + yhat_unseen20.7))
    #n20.8.outsample <- caret::confusionMatrix(xtabs(~test.y + yhat_unseen20.8))
    #n20.9.outsample <- caret::confusionMatrix(xtabs(~test.y + yhat_unseen20.9))
    #n20.10.outsample <- caret::confusionMatrix(xtabs(~test.y + yhat_unseen20.10))

    #shrinkage <- rbind(
    #  cbind(Size = 20.0, Sample = "In", as.data.frame(t(n20.0.insample$overall[measures]))),
    #  cbind(Size = 20.0, Sample = "Out", as.data.frame(t(n20.0.outsample$overall[measures]))),
    #  cbind(Size = 20.1, Sample = "In", as.data.frame(t(n20.1.insample$overall[measures]))),
    #  cbind(Size = 20.1, Sample = "Out", as.data.frame(t(n20.1.outsample$overall[measures]))),
    #  cbind(Size = 20.2, Sample = "In", as.data.frame(t(n20.2.insample$overall[measures]))),
    #  cbind(Size = 20.2, Sample = "Out", as.data.frame(t(n20.2.outsample$overall[measures]))),
    #  cbind(Size = 20.3, Sample = "In", as.data.frame(t(n20.3.insample$overall[measures]))),
    #  cbind(Size = 20.3, Sample = "Out", as.data.frame(t(n20.3.outsample$overall[measures]))),
    #  cbind(Size = 20.4, Sample = "In", as.data.frame(t(n20.4.insample$overall[measures]))),
    #  cbind(Size = 20.4, Sample = "Out", as.data.frame(t(n20.4.outsample$overall[measures]))),
    #  cbind(Size = 20.5, Sample = "In", as.data.frame(t(n20.5.insample$overall[measures]))),
    #  cbind(Size = 20.5, Sample = "Out", as.data.frame(t(n20.5.outsample$overall[measures]))),
    #  cbind(Size = 20.6, Sample = "In", as.data.frame(t(n20.6.insample$overall[measures]))),
    #  cbind(Size = 20.6, Sample = "Out", as.data.frame(t(n20.6.outsample$overall[measures]))),
    #  cbind(Size = 20.7, Sample = "In", as.data.frame(t(n20.7.insample$overall[measures]))),
    #  cbind(Size = 20.7, Sample = "Out", as.data.frame(t(n20.7.outsample$overall[measures]))),
    #  cbind(Size = 20.8, Sample = "In", as.data.frame(t(n20.8.insample$overall[measures]))),
    #  cbind(Size = 20.8, Sample = "Out", as.data.frame(t(n20.8.outsample$overall[measures]))),
    #  cbind(Size = 20.9, Sample = "In", as.data.frame(t(n20.9.insample$overall[measures]))),
    #  cbind(Size = 20.9, Sample = "Out", as.data.frame(t(n20.9.outsample$overall[measures]))),
    #  cbind(Size = 20.99, Sample = "In", as.data.frame(t(n20.10.insample$overall[measures]))),
    #  cbind(Size = 20.99, Sample = "Out", as.data.frame(t(n20.10.outsample$overall[measures])))
    #  )
    #shrinkage$Pkg <- rep(c("In", "Out"), 1)

    #dodge <- position_dodge(width=0.4)

    #p.shrinkage <- ggplot(shrinkage, aes(interaction(Size, sep = " : "), Accuracy,
    #                      ymin = AccuracyLower, ymax = AccuracyUpper,
    #                      shape = Sample, linetype = Sample)) +
    #  geom_point(size = 2.5, position = dodge) +
    #  geom_errorbar(width = .25, position = dodge) +
    #  xlab("") + ylab("Accuracy + 95% CI") +
    #  theme_classic() +
    #  theme(legend.key.size = unit(1, "cm"), legend.position = c(.8, .2))

    #png("test_20.png",
    #    width = 6, height = 6, units = "in", res = 600)
    #  print(p.shrinkage)
    #dev.off()

    ##################################################################
    # Neural Net: Models for Plotting Tuning Differences (Part 2)
    ##################################################################

    # This time I am tuning the hidden layer size, ranging from 5 
    # hidden neurons to 100.

    #m5 <- train(x = train.X, y = train.y,
    #           method = "nnet", 
    #           tuneGrid = expand.grid(
    #           .size = c(5),
    #           .decay = 0),
    #           trControl= trainControl(method = "none"),
    #           MaxNWts = 2000,
    #           maxit = 100)
    #m10 <- train(x = train.X, y = train.y,
    #           method = "nnet", 
    #           tuneGrid = expand.grid(
    #           .size = c(10),
    #           .decay = 0),
    #           trControl= trainControl(method = "none"),
    #           MaxNWts = 2000,
    #           maxit = 100)
    #m15 <- train(x = train.X, y = train.y,
    #           method = "nnet", 
    #           tuneGrid = expand.grid(
    #           .size = c(15),
    #           .decay = 0),
    #           trControl= trainControl(method = "none"),
    #           MaxNWts = 2000,
    #           maxit = 100)
    #m20 <- train(x = train.X, y = train.y,
    #           method = "nnet", 
    #           tuneGrid = expand.grid(
    #           .size = c(20),
    #           .decay = 0),
    #           trControl= trainControl(method = "none"),
    #           MaxNWts = 2000,
    #           maxit = 100)
    #m30 <- train(x = train.X, y = train.y,
    #           method = "nnet", 
    #           tuneGrid = expand.grid(
    #           .size = c(30),
    #           .decay = 0),
    #           trControl= trainControl(method = "none"),
    #           MaxNWts = 2000,
    #           maxit = 100)
    #m40 <- train(x = train.X, y = train.y,
    #           method = "nnet", 
    #           tuneGrid = expand.grid(
    #           .size = c(40),
    #           .decay = 0),
    #           trControl= trainControl(method = "none"),
    #           MaxNWts = 2000,
    #           maxit = 100)
    #m50 <- train(x = train.X, y = train.y,
    #           method = "nnet", 
    #           tuneGrid = expand.grid(
    #           .size = c(50),
    #           .decay = 0),
    #           trControl= trainControl(method = "none"),
    #           MaxNWts = 2000,
    #           maxit = 100)
    #m70 <- train(x = train.X, y = train.y,
    #           method = "nnet", 
    #           tuneGrid = expand.grid(
    #           .size = c(70),
    #           .decay = 0),
    #           trControl= trainControl(method = "none"),
    #           MaxNWts = 2000,
    #           maxit = 100)
    #m100 <- train(x = train.X, y = train.y,
    #           method = "nnet", 
    #           tuneGrid = expand.grid(
    #           .size = c(100),
    #           .decay = 0),
    #           trControl= trainControl(method = "none"),
    #           MaxNWts = 20000,
    #           maxit = 100)

    #yhat5 <- predict(m5)
    #yhat10 <- predict(m10)
    #yhat15 <- predict(m15)
    #yhat20 <- predict(m20)
    #yhat30 <- predict(m30)
    #yhat40 <- predict(m40)
    #yhat50 <- predict(m50)
    #yhat70 <- predict(m70)
    #yhat100 <- predict(m100)

    #yhat_unseen5 <- predict(m5, as.matrix(test.X))
    #yhat_unseen10 <- predict(m10, as.matrix(test.X))
    #yhat_unseen15 <- predict(m15, as.matrix(test.X))
    #yhat_unseen20 <- predict(m20, as.matrix(test.X))
    #yhat_unseen30 <- predict(m30, as.matrix(test.X))
    #yhat_unseen40 <- predict(m40, as.matrix(test.X))
    #yhat_unseen50 <- predict(m50, as.matrix(test.X))
    #yhat_unseen70 <- predict(m70, as.matrix(test.X))
    #yhat_unseen100 <- predict(m100, as.matrix(test.X))

    #measures <- c("AccuracyNull", "Accuracy", "AccuracyLower", "AccuracyUpper")

    #n5.insample <- caret::confusionMatrix(xtabs(~train.y + yhat5))
    #n10.insample <- caret::confusionMatrix(xtabs(~train.y + yhat10))
    #n15.insample <- caret::confusionMatrix(xtabs(~train.y + yhat15))
    #n20.insample <- caret::confusionMatrix(xtabs(~train.y + yhat20))
    #n30.insample <- caret::confusionMatrix(xtabs(~train.y + yhat30))
    #n40.insample <- caret::confusionMatrix(xtabs(~train.y + yhat40))
    #n50.insample <- caret::confusionMatrix(xtabs(~train.y + yhat50))
    #n70.insample <- caret::confusionMatrix(xtabs(~train.y + yhat70))
    #n100.insample <- caret::confusionMatrix(xtabs(~train.y + yhat100))

    #n5.outsample <- caret::confusionMatrix(xtabs(~test.y + yhat_unseen5))
    #n10.outsample <- caret::confusionMatrix(xtabs(~test.y + yhat_unseen10))
    #n15.outsample <- caret::confusionMatrix(xtabs(~test.y + yhat_unseen15))
    #n20.outsample <- caret::confusionMatrix(xtabs(~test.y + yhat_unseen20))
    #n30.outsample <- caret::confusionMatrix(xtabs(~test.y + yhat_unseen30))
    #n40.outsample <- caret::confusionMatrix(xtabs(~test.y + yhat_unseen40))
    #n50.outsample <- caret::confusionMatrix(xtabs(~test.y + yhat_unseen50))
    #n70.outsample <- caret::confusionMatrix(xtabs(~test.y + yhat_unseen70))
    #n100.outsample <- caret::confusionMatrix(xtabs(~test.y + yhat_unseen100))

    #shrinkage <- rbind(
    #  cbind(Size = 5, Sample = "In", as.data.frame(t(n5.insample$overall[measures]))),
    #  cbind(Size = 5, Sample = "Out", as.data.frame(t(n5.outsample$overall[measures]))),
    #  cbind(Size = 10, Sample = "In", as.data.frame(t(n10.insample$overall[measures]))),
    #  cbind(Size = 10, Sample = "Out", as.data.frame(t(n10.outsample$overall[measures]))),
    #  cbind(Size = 15, Sample = "In", as.data.frame(t(n15.insample$overall[measures]))),
    #  cbind(Size = 15, Sample = "Out", as.data.frame(t(n15.outsample$overall[measures]))),
    #  cbind(Size = 20, Sample = "In", as.data.frame(t(n20.insample$overall[measures]))),
    #  cbind(Size = 20, Sample = "Out", as.data.frame(t(n20.outsample$overall[measures]))),
    #  cbind(Size = 30, Sample = "In", as.data.frame(t(n30.insample$overall[measures]))),
    #  cbind(Size = 30, Sample = "Out", as.data.frame(t(n30.outsample$overall[measures]))),
    #  cbind(Size = 40, Sample = "In", as.data.frame(t(n40.insample$overall[measures]))),
    #  cbind(Size = 40, Sample = "Out", as.data.frame(t(n40.outsample$overall[measures]))),
    #  cbind(Size = 50, Sample = "In", as.data.frame(t(n50.insample$overall[measures]))),
    #  cbind(Size = 50, Sample = "Out", as.data.frame(t(n50.outsample$overall[measures]))),
    #  cbind(Size = 70, Sample = "In", as.data.frame(t(n70.insample$overall[measures]))),
    #  cbind(Size = 70, Sample = "Out", as.data.frame(t(n70.outsample$overall[measures]))),
    #  cbind(Size = 100, Sample = "In", as.data.frame(t(n100.insample$overall[measures]))),
    #  cbind(Size = 100, Sample = "Out", as.data.frame(t(n100.outsample$overall[measures])))
    #  )
    #shrinkage$Pkg <- rep(c("In", "Out"), 1)

    #dodge <- position_dodge(width=0.4)

    #p.shrinkage <- ggplot(shrinkage, aes(interaction(Size, sep = " : "), Accuracy,
    #                      ymin = AccuracyLower, ymax = AccuracyUpper,
    #                      shape = Sample, linetype = Sample)) +
    #  geom_point(size = 2.5, position = dodge) +
    #  geom_errorbar(width = .25, position = dodge) +
    #  xlab("") + ylab("Accuracy + 95% CI") +
    #  theme_classic() +
    #  theme(legend.key.size = unit(1, "cm"), legend.position = c(.8, .2))

    #png("test_5150.png",
    #    width = 6, height = 6, units = "in", res = 600)
    #  print(p.shrinkage)
    #dev.off()

    ##################################################################
    # Neural Net: H2O Models
    ##################################################################

    # Initializing the H2O-cluster
    c1 <- h2o.init()

    # Setting up the data for H2O
    train$volact <- as.factor(train$volact)
    test$volact <- as.factor(test$volact)

    h2o.train <- as.h2o(train)
    h2o.test <- as.h2o(test)

    train.bal$volact <- as.factor(train.bal$volact)
    h2o.train.bal <- as.h2o(train.bal)

    xnames <- colnames(train[,-1])

    ##################################################################
    # Neural Net: H2O Models for Plotting Tuning Differences (Part 3)
    ##################################################################

    # Now I am testing different depths of hidden layers, starting 
    # with one layer and going until 4 layers. The error 
    # distribution is going to be plotted. 

    #m2a <- h2o.deeplearning(
    #  x = xnames,
    #  #y = "Outcome",
    #  training_frame= h2o.train,
    #  validation_frame = h2o.test,
    #  activation = "Tanh",
    #  autoencoder = TRUE,
    #  hidden = c(20),
    #  epochs = 10,
    #  sparsity_beta = 0,
    #  l1 = 0,
    #  l2 = 0
    #)

    #m2b <- h2o.deeplearning(
    #  x = xnames,
    #  #y = "Outcome",
    #  training_frame= h2o.train,
    #  validation_frame = h2o.test,
    #  activation = "Tanh",
    #  autoencoder = TRUE,
    #  hidden = c(20, 15),
    #  epochs = 10,
    #  sparsity_beta = 0,
    #  #hidden_pout_ratios = c(.3),
    #  l1 = 0,
    #  l2 = 0
    #)

    #m2c <- h2o.deeplearning(
    #  x = xnames,
    #  #y = "Outcome",
    #  training_frame= h2o.train,
    #  validation_frame = h2o.test,
    #  activation = "Tanh",
    #  autoencoder = TRUE,
    #  hidden = c(20, 15, 10),
    #  epochs = 10,
    #  sparsity_beta = 0,
    #  l1 = 0,
    #  l2 = 0
    #)

    #m2d <- h2o.deeplearning(
    #  x = xnames,
    #  #y = "Outcome",
    #  training_frame= h2o.train,
    #  validation_frame = h2o.test,
    #  activation = "Tanh",
    #  autoencoder = TRUE,
    #  hidden = c(20, 15, 10, 5),
    #  epochs = 10,
    #  sparsity_beta = 0,
    #  #hi2den_dropout_ratios = c(.3),
    #  l1 = 0,
    #  l2 = 0
    #)

    #summary(m2a)
    #summary(m2b)
    #summary(m2c)
    #summary(m2d)

    #error1 <- as.data.frame(h2o.anomaly(m2a, h2o.train))
    #error2a <- as.data.frame(h2o.anomaly(m2b, h2o.train))
    #error2b <- as.data.frame(h2o.anomaly(m2c, h2o.train))
    #error2c <- as.data.frame(h2o.anomaly(m2d, h2o.train))

    #error <- as.data.table(rbind(
    #  cbind.data.frame(Model = "2a", error1),
    #  cbind.data.frame(Model = "2b", error2a),
    #  cbind.data.frame(Model = "2c", error2b),
    #  cbind.data.frame(Model = "2d", error2c)))

    #percentile <- error[, .(
    #  Percentile = quantile(Reconstruction.MSE, probs = .95)
    #), by = Model]

    #p1 <- ggplot(error, aes(Reconstruction.MSE)) +
    #  geom_histogram(binwidth = .001, fill = "grey50") +
    #  geom_vline(aes(xintercept = Percentile), data = percentile, linetype = 2) +
    #  theme_bw() +
    #  facet_wrap(~Model)
    #print(p1)

    #png("error_1.png",
    #    width = 5.5, height = 5.5, units = "in", res = 600)
    #print(p1)
    #dev.off()

    ##################################################################
    # Neural Net: Calculated on the "Basic Model" Data
    ##################################################################

    mt3 <- h2o.deeplearning(
      x = xnames,
      y = "volact",
      training_frame = h2o.train,
      validation_frame = h2o.test,
      activation = "Rectifier",
      hidden = c(200, 200, 100),
      epochs = 10,
      rate = .005,
      loss = "CrossEntropy",
      #input_dropout_ratio = .2,
      #hidden_dropout_ratios = c(.5, .3, .1),
      export_weights_and_biases = TRUE
    )
    summary(mt3)

    mt3_pred <- h2o.predict(mt3, h2o.test)
    ACC(table(as.vector(mt3_pred$predict), as.vector(h2o.test$volact)))
    F1(table(as.vector(mt3_pred$predict), as.vector(h2o.test$volact)))

    # These variable importances are used for the diagramm.
    #h2o.varimp(mt3)

    ##################################################################
    # Neural Net: Calculated on Balanced Data
    ##################################################################

    mtbal <- h2o.deeplearning(
      x = xnames,
      y = "volact",
      training_frame = h2o.train.bal,
      validation_frame = h2o.test,
      activation = "Rectifier",
      hidden = c(200, 200, 100),
      epochs = 10,
      rate = .005,
      loss = "CrossEntropy",
      #input_dropout_ratio = .2,
      #hidden_dropout_ratios = c(.5, .3, .1),
      export_weights_and_biases = TRUE
    )
    summary(mtbal)

    mtbal_pred <- h2o.predict(mtbal, h2o.test)
    ACC(table(as.vector(mtbal_pred$predict), as.vector(h2o.test$volact)))
    F1(table(as.vector(mtbal_pred$predict), as.vector(h2o.test$volact)))

    #h2o.varimp(mtbal)

    ##################################################################
    # Neural Net: Plot a Heatmap of the First Layer Weights
    ##################################################################

    ## weights for mapping from inputs to hidden layer 1 neurons
    #w1 <- as.matrix(h2o.weights(mtbal, 1))

    ## plot heatmap of the weights
    #tmp <- as.data.frame(t(w1))
    #tmp$Row <- 1:nrow(tmp)
    #tmp <- melt(tmp, id.vars = c("Row"))

    #p.heat <- ggplot(tmp,
    #       aes(variable, Row, fill = value)) +
    #  geom_tile() +
    #  scale_fill_gradientn(colours = c("black", "white", "blue")) +
    #  theme_classic() +
    #  theme(axis.text = element_blank()) +
    #  xlab("Hidden Neuron") +
    #  ylab("Input Variable") +
    #  ggtitle("Heatmap of Weights for Layer 1")
    #print(p.heat)

    #png("heatmap_layer1.png",
    #    width = 5.5, height = 7.5, units = "in", res = 600)
    #print(p.heat)
    #dev.off()

    ##################################################################
    # Neural Net: Preparing Plot for Marginal Effects later
    ##################################################################

    h2o.plot <- as.h2o(plot.data)

    mt3_pred <- h2o.predict(mt3, h2o.plot)

    ##################################################################
    # Random Forest: Calculated on the "Basic Model" Data
    ##################################################################

    rf2 <- h2o.randomForest(        
      training_frame = h2o.train,        
      validation_frame = h2o.test,      
      x = xnames,                        
      y = "volact",                          
      model_id = "forest2",    
      ntrees = 400,            
      max_depth = 30,
      stopping_rounds = 0,           
      score_each_iteration = F,     
      seed = 1000000) 
    summary(rf2)

    # These variable importances are used for the diagramm.
    # h2o.varimp(rf2)

    rf2_pred <- h2o.predict(rf2, h2o.test)
    ACC(table(as.vector(rf2_pred$predict), as.vector(h2o.test$volact)))
    F1(table(as.vector(rf2_pred$predict), as.vector(h2o.test$volact)))

    ##################################################################
    # Random Forest: Calculated on Balanced Data
    ##################################################################

    rfbal <- h2o.randomForest(        
      training_frame = h2o.train.bal,        
      validation_frame = h2o.test,      
      x = xnames,                        
      y = "volact",                          
      model_id = "forestbal",    
      ntrees = 400,            
      max_depth = 30,
      stopping_rounds = 0,           
      score_each_iteration = F,     
      seed = 1000000) 
    summary(rfbal)

    rfbal_pred <- h2o.predict(rfbal, h2o.test)
    ACC(table(as.vector(rfbal_pred$predict), as.vector(h2o.test$volact)))
    F1(table(as.vector(rfbal_pred$predict), as.vector(h2o.test$volact)))


    ##################################################################
    # Random Forest: Preparing Plot for Marginal Effects
    ##################################################################

    rf2_pred <- h2o.predict(rf2, h2o.plot)

    # Condensing the prepared results into one dataframe 
    # and plotting it.

    age.volact <- data.frame(cbind(as.matrix(age.volact), 
        as.vector(mt3_pred$p1), as.vector(rf2_pred$p1)))
    colnames(age.volact) <- c("range", "1", "2", "3")
    melted <- melt(age.volact, id.vars="range")

    #png("age-volact.png")
    #myplot <- ggplot(data=melted, aes(x=range, y=value, group=variable)) + 
    #    geom_line(col = melted$variable) +
    #    ylab("Predicted Probability of Voluntary Action") + 
    #    xlab("Year of Birth (Scaled)") + 
    #    ggtitle("Comparison: \n Predicted Probabilites of Voluntary Action for 
    #    a range of Year of Births \n From top to down: Logistic Regression, Neural Network, Random Forest") + 
    #    theme_bw() 
    #print(myplot)
    #dev.off()
    #print(myplot)

    ##################################################################
    ##################################################################

    ##################################################################
    # Preparing the Data for the Separate Models
    ##################################################################

    ess1 <- read.table("C:/Users/laris/Desktop/MMDS/Semester3-FSS2018/Advanced-Quantitative-Methods/paper/Submission/ess1.csv", header=TRUE, sep=",") 

    ess6 <- read.table("C:/Users/laris/Desktop/MMDS/Semester3-FSS2018/Advanced-Quantitative-Methods/paper/Submission/ess", header=TRUE, sep=",") 

    for(i in c(4,5,6,10,11,12,13,14,15,16,17,18,19,20,21,22,23)){
        ess1[is.na(ess1[,i]), i] <- mean(ess1[,i], na.rm = TRUE)
    }

    ess1[is.na(ess1[,3]), 3] <- 1
    ess1[is.na(ess1[,7]), 7] <- 1
    ess1[is.na(ess1[,8]), 8] <- 0
    ess1[is.na(ess1[,9]), 9] <- 0

    for(i in c(4,5,6,10,11,12,13,14,15,16,17,18,19,20,21,22,23)){
        ess6[is.na(ess6[,i]), i] <- mean(ess6[,i], na.rm = TRUE)
    }

    ess6[is.na(ess6[,3]), 3] <- 1
    ess6[is.na(ess6[,7]), 7] <- 1
    ess6[is.na(ess6[,8]), 8] <- 0
    ess6[is.na(ess6[,9]), 9] <- 0

    ess1.scaled <- cbind(ess1[c(1, 3, 7, 8, 9)], scale(ess1[-c(1, 2, 3, 7, 8, 9, 24)]))

    ess1c.scaled <- cbind(ess1[c(1, 2, 3, 7, 8, 9)], scale(ess1[-c(1, 2, 3, 7, 8, 9, 24)]))

    ess6.scaled <- cbind(ess6[c(1, 3, 7, 8, 9)], scale(ess6[-c(1, 2, 3, 7, 8, 9, 24)]))

    ess6c.scaled <- cbind(ess6[c(1, 2, 3, 7, 8, 9)], scale(ess6[-c(1, 2, 3, 7, 8, 9, 24)]))

    for(t in unique(ess1c.scaled$cntry)) {
        ess1c.scaled[paste("cntry",t,sep="")] <- ifelse(ess1c.scaled$cntry==t,1,0)
    }

    ess1c.scaled <- ess1c.scaled[,-2]

    for(t in unique(ess6c.scaled$cntry)) {
        ess6c.scaled[paste("cntry",t,sep="")] <- ifelse(ess6c.scaled$cntry==t,1,0)
    }

    ess6c.scaled <- ess6c.scaled[,-2]

    # Same as above: this can be un-commented when the data sets are 
    # already there and just have to be written and loaded.

    #write.csv(ess1.scaled, file = "ess1-scaled.csv")
    #write.csv(ess1c.scaled, file = "ess1-scaled-cntry.csv")

    #write.csv(ess6.scaled, file = "ess6-scaled.csv")
    #write.csv(ess6c.scaled, file = "ess6-scaled-cntry.csv")

    #ess1.scaled <- read.table("ess1-scaled.csv", header=TRUE, sep=",") 
    #ess6.scaled <- read.table("ess6-scaled.csv", header=TRUE, sep=",") 
    #ess1c.scaled <- read.table("ess1-scaled-cntry.csv", header=TRUE, sep=",") 
    #ess6c.scaled <- read.table("ess6-scaled-cntry.csv", header=TRUE, sep=",") 

    #ess1.scaled <- ess1.scaled[,-1]
    #ess6.scaled <- ess6.scaled[,-1]

    #ess1c.scaled <- ess1c.scaled[,-1]
    #ess6c.scaled <- ess6c.scaled[,-1]

    # Again: splitting into training and test set

    set.seed(123)
    smp_size <- floor(0.9 * nrow(ess1.scaled))
    train_ind <- sample(seq_len(nrow(ess1.scaled)), size = smp_size)
    train1 <- ess1.scaled[train_ind,]
    test1 <- ess1.scaled[-train_ind,]
    train1c <- ess1c.scaled[train_ind,]
    test1c <- ess1c.scaled[-train_ind,]

    set.seed(123)
    smp_size <- floor(0.9 * nrow(ess6.scaled))
    train_ind <- sample(seq_len(nrow(ess6.scaled)), size = smp_size)
    train6 <- ess6.scaled[train_ind,]
    test6 <- ess6.scaled[-train_ind,]
    train6c <- ess6c.scaled[train_ind,]
    test6c <- ess6c.scaled[-train_ind,]

    ##################################################################
    # Logistic Regression: Calculated on two years separately
    ##################################################################

    y1 <- train1$volact
    X1 <- as.matrix(train1[,-1])
    ols_model1 <- glm(y1 ~ X1, family=binomial(link='logit'))
    X1 <- as.matrix(test1[,-1])
    p_ols1 <- predict(ols_model1, data.frame(X1), type = "response")

    #table(p_ols1, test1$volact)
    #ACC(table(p_ols1, test1$volact))
    #F1(table(p_ols1, test1$volact))

    y6 <- train6$volact
    X6 <- as.matrix(train6[,-1])
    ols_model6 <- glm(y6 ~ X6, family=binomial(link='logit'))
    X6 <- as.matrix(test6[,-1])
    p_ols6 <- predict(ols_model6, data.frame(X6), type = "response")

    #table(y_hat6, test6$volact)
    #ACC(table(y_hat6, test6$volact))
    #F1(table(y_hat6, test6$volact))

    p_ols11 <- ifelse(p_ols1 > 0.1, 1, 0)
    p_ols12 <- ifelse(p_ols1 > 0.2, 1, 0)
    p_ols13 <- ifelse(p_ols1 > 0.3, 1, 0)
    p_ols14 <- ifelse(p_ols1 > 0.4, 1, 0)
    p_ols15 <- ifelse(p_ols1 > 0.5, 1, 0)
    p_ols16 <- ifelse(p_ols1 > 0.6, 1, 0)
    p_ols17 <- ifelse(p_ols1 > 0.7, 1, 0)
    #p_ols18 <- ifelse(p_ols1 > 0.8, 1, 0)
    #p_ols19 <- ifelse(p_ols1 > 0.9, 1, 0)

    threshold <- cbind(seq(0.1, 0.7, length.out = 7),
        
    c(ACC(table(p_ols11, test1$volact)),
    ACC(table(p_ols12, test1$volact)),
    ACC(table(p_ols13, test1$volact)),
    ACC(table(p_ols14, test1$volact)),
    ACC(table(p_ols15, test1$volact)),
    ACC(table(p_ols16, test1$volact)),
    ACC(table(p_ols17, test1$volact))),
    #ACC(table(p_ols18, test1$volact)),
    #ACC(table(p_ols19, test1$volact))),

    c(F1(table(p_ols11, test1$volact)),
    F1(table(p_ols12, test1$volact)),
    F1(table(p_ols13, test1$volact)),
    F1(table(p_ols14, test1$volact)),
    F1(table(p_ols15, test1$volact)),
    F1(table(p_ols16, test1$volact)),
    F1(table(p_ols17, test1$volact))))
    #F1(table(p_ols18, test1$volact))))
    #F1(table(p_ols19, test1$volact))))

    print("MAX Accuracy for ESS1 Logistic Regression")
    max(threshold[,2])
    print("MAX F1 for ESS1 Logistic Regression")
    max(threshold[,3])

    p_ols61 <- ifelse(p_ols6 > 0.1, 1, 0)
    p_ols62 <- ifelse(p_ols6 > 0.2, 1, 0)
    p_ols63 <- ifelse(p_ols6 > 0.3, 1, 0)
    p_ols64 <- ifelse(p_ols6 > 0.4, 1, 0)
    p_ols65 <- ifelse(p_ols6 > 0.5, 1, 0)
    p_ols66 <- ifelse(p_ols6 > 0.6, 1, 0)
    p_ols67 <- ifelse(p_ols6 > 0.7, 1, 0)
    p_ols68 <- ifelse(p_ols6 > 0.8, 1, 0)
    #p_ols69 <- ifelse(p_ols6 > 0.9, 1, 0)

    threshold <- cbind(seq(0.1, 0.8, length.out = 8),
        
    c(ACC(table(p_ols61, test6$volact)),
    ACC(table(p_ols62, test6$volact)),
    ACC(table(p_ols63, test6$volact)),
    ACC(table(p_ols64, test6$volact)),
    ACC(table(p_ols65, test6$volact)),
    ACC(table(p_ols66, test6$volact)),
    ACC(table(p_ols67, test6$volact)),
    ACC(table(p_ols68, test6$volact))),
    #ACC(table(p_ols69, test6$volact))),

    c(F1(table(p_ols61, test6$volact)),
    F1(table(p_ols62, test6$volact)),
    F1(table(p_ols63, test6$volact)),
    F1(table(p_ols64, test6$volact)),
    F1(table(p_ols65, test6$volact)),
    F1(table(p_ols66, test6$volact)),
    F1(table(p_ols67, test6$volact)),
    F1(table(p_ols68, test6$volact))))
    #F1(table(p_ols69, test6$volact))))

    print("MAX Accuracy for ESS6 Logistic Regression")
    max(threshold[,2])
    print("MAX F1 for ESS6 Logistic Regression")
    max(threshold[,3])

    ##################################################################
    # Logistic Regression: Calculated with Country Dummies
    ##################################################################

    y1c <- train1c$volact
    X1c <- as.matrix(train1c[,-c(1, 44)])
    ols_model1c <- glm(y1c ~ X1c, family=binomial(link='logit'))
    X1c <- as.matrix(test1c[,-c(1, 44)])
    p_ols1c <- predict(ols_model1c, data.frame(X1c), type = "response")

    #table(p_ols1, test1$volact)
    #ACC(table(p_ols1, test1$volact))
    #F1(table(p_ols1, test1$volact))

    y6c <- train6c$volact
    X6c <- as.matrix(train6c[,-c(1, 51)])
    ols_model6c <- glm(y6c ~ X6c, family=binomial(link='logit'))
    X6c <- as.matrix(test6c[,-c(1, 51)])
    p_ols6c <- predict(ols_model6c, data.frame(X6c), type = "response")

    #table(y_hat6, test6$volact)
    #ACC(table(y_hat6, test6$volact))
    #F1(table(y_hat6, test6$volact))

    p_ols11c <- ifelse(p_ols1c > 0.1, 1, 0)
    p_ols12c <- ifelse(p_ols1c > 0.2, 1, 0)
    p_ols13c <- ifelse(p_ols1c > 0.3, 1, 0)
    p_ols14c <- ifelse(p_ols1c > 0.4, 1, 0)
    p_ols15c <- ifelse(p_ols1c > 0.5, 1, 0)
    p_ols16c <- ifelse(p_ols1c > 0.6, 1, 0)
    p_ols17c <- ifelse(p_ols1c > 0.7, 1, 0)
    p_ols18c <- ifelse(p_ols1c > 0.8, 1, 0)
    #p_ols19c <- ifelse(p_ols1c > 0.9, 1, 0)

    threshold <- cbind(seq(0.1, 0.8, length.out = 8),
        
    c(ACC(table(p_ols11c, test1c$volact)),
    ACC(table(p_ols12c, test1c$volact)),
    ACC(table(p_ols13c, test1c$volact)),
    ACC(table(p_ols14c, test1c$volact)),
    ACC(table(p_ols15c, test1c$volact)),
    ACC(table(p_ols16c, test1c$volact)),
    ACC(table(p_ols17c, test1c$volact)),
    ACC(table(p_ols18c, test1c$volact))),
    #ACC(table(p_ols19c, test1c$volact))),

    c(F1(table(p_ols11c, test1c$volact)),
    F1(table(p_ols12c, test1c$volact)),
    F1(table(p_ols13c, test1c$volact)),
    F1(table(p_ols14c, test1c$volact)),
    F1(table(p_ols15c, test1c$volact)),
    F1(table(p_ols16c, test1c$volact)),
    F1(table(p_ols17c, test1c$volact)),
    F1(table(p_ols18c, test1c$volact))))
    #F1(table(p_ols19c, test1c$volact))))

    print("MAX Accuracy for ESS1 Logistic Regression with Countries")
    max(threshold[,2])
    print("MAX F1 for ESS1 Logistic Regression with Countries")
    max(threshold[,3])

    p_ols61c <- ifelse(p_ols6c > 0.1, 1, 0)
    p_ols62c <- ifelse(p_ols6c > 0.2, 1, 0)
    p_ols63c <- ifelse(p_ols6c > 0.3, 1, 0)
    p_ols64c <- ifelse(p_ols6c > 0.4, 1, 0)
    p_ols65c <- ifelse(p_ols6c > 0.5, 1, 0)
    p_ols66c <- ifelse(p_ols6c > 0.6, 1, 0)
    p_ols67c <- ifelse(p_ols6c > 0.7, 1, 0)
    p_ols68c <- ifelse(p_ols6c > 0.8, 1, 0)
    #p_ols69c <- ifelse(p_ols6c > 0.9, 1, 0)

    threshold <- cbind(seq(0.1, 0.8, length.out = 8),
        
    c(ACC(table(p_ols61c, test6c$volact)),
    ACC(table(p_ols62c, test6c$volact)),
    ACC(table(p_ols63c, test6c$volact)),
    ACC(table(p_ols64c, test6c$volact)),
    ACC(table(p_ols65c, test6c$volact)),
    ACC(table(p_ols66c, test6c$volact)),
    ACC(table(p_ols67c, test6c$volact)),
    ACC(table(p_ols68c, test6c$volact))),
    #ACC(table(p_ols69c, test6c$volact))),

    c(F1(table(p_ols61c, test6c$volact)),
    F1(table(p_ols62c, test6c$volact)),
    F1(table(p_ols63c, test6c$volact)),
    F1(table(p_ols64c, test6c$volact)),
    F1(table(p_ols65c, test6c$volact)),
    F1(table(p_ols66c, test6c$volact)),
    F1(table(p_ols67c, test6c$volact)),
    F1(table(p_ols68c, test6c$volact))))
    #F1(table(p_ols69c, test6c$volact))))

    print("MAX Accuracy for ESS6 Logistic Regression with Countries")
    max(threshold[,2])
    print("MAX F1 for ESS6 Logistic Regression with Countries")
    max(threshold[,3])

    ##################################################################
    # Neural Net: H2O Models Extended
    ##################################################################

    # Again: transforming the data into H2O-data
    train1$volact <- as.factor(train1$volact)
    test1$volact <- as.factor(test1$volact)
    train6$volact <- as.factor(train6$volact)
    test6$volact <- as.factor(test6$volact)

    h2o.train1 <- as.h2o(train1)
    h2o.test1 <- as.h2o(test1)
    h2o.train6 <- as.h2o(train6)
    h2o.test6 <- as.h2o(test6)

    train1c$volact <- as.factor(train1c$volact)
    test1c$volact <- as.factor(test1c$volact)
    train6c$volact <- as.factor(train6c$volact)
    test6c$volact <- as.factor(test6c$volact)

    h2o.train1c <- as.h2o(train1c)
    h2o.test1c <- as.h2o(test1c)
    h2o.train6c <- as.h2o(train6c)
    h2o.test6c <- as.h2o(test6c)

    xnames1 <- colnames(train1[,-1])
    xnames6 <- colnames(train6[,-1])

    xnames1c <- colnames(train1c[,-1])
    xnames6c <- colnames(train6c[,-1])

    ##################################################################
    # Neural Net: Calculated on two years separately
    ##################################################################

    nn1_1 <- h2o.deeplearning(
      x = xnames1,
      y = "volact",
      training_frame = h2o.train1,
      validation_frame = h2o.test1,
      activation = "Rectifier",
      hidden = c(200, 200, 100),
      epochs = 10,
      rate = .005,
      loss = "CrossEntropy",
      export_weights_and_biases = TRUE,
      seed = 1000000
    )

    nn6_1 <- h2o.deeplearning(
      x = xnames6,
      y = "volact",
      training_frame = h2o.train6,
      validation_frame = h2o.test6,
      activation = "Rectifier",
      hidden = c(200, 200, 100),
      epochs = 10,
      rate = .005,
      loss = "CrossEntropy",
      export_weights_and_biases = TRUE, 
      seed = 1000000
    )

    summary(nn1_1)

    summary(nn6_1)

    nn1_1_pred <- h2o.predict(nn1_1, h2o.test1)
    nn6_1_pred <- h2o.predict(nn6_1, h2o.test6)
    ACC(table(as.vector(nn1_1_pred$predict), test1$volact))
    F1(table(as.vector(nn1_1_pred$predict), test1$volact))
    ACC(table(as.vector(nn6_1_pred$predict), test6$volact))
    F1(table(as.vector(nn6_1_pred$predict), test6$volact))

    ##################################################################
    # Neural Net: Calculated with Country Dummies
    ##################################################################

    nn1_1c <- h2o.deeplearning(
      x = xnames1c,
      y = "volact",
      training_frame = h2o.train1c,
      validation_frame = h2o.test1c,
      activation = "Rectifier",
      hidden = c(200, 200, 100),
      epochs = 10,
      rate = .005,
      loss = "CrossEntropy",
      export_weights_and_biases = TRUE,
      seed = 1000000
    )

    nn6_1c <- h2o.deeplearning(
      x = xnames6c,
      y = "volact",
      training_frame = h2o.train6c,
      validation_frame = h2o.test6c,
      activation = "Rectifier",
      hidden = c(200, 200, 100),
      epochs = 10,
      rate = .005,
      loss = "CrossEntropy",
      export_weights_and_biases = TRUE, 
      seed = 1000000
    )

    summary(nn1_1c)

    summary(nn6_1c)

    nn1_1c_pred <- h2o.predict(nn1_1c, h2o.test1c)
    nn6_1c_pred <- h2o.predict(nn6_1c, h2o.test6c)
    ACC(table(as.vector(nn1_1c_pred$predict), test1c$volact))
    F1(table(as.vector(nn1_1c_pred$predict), test1c$volact))
    ACC(table(as.vector(nn6_1c_pred$predict), test6c$volact))
    F1(table(as.vector(nn6_1c_pred$predict), test6c$volact))

    ##################################################################
    # Random Forest: Calculated on two years separately
    ##################################################################

    rf1_1 <- h2o.randomForest(        
      training_frame = h2o.train1,        
      validation_frame = h2o.test1,      
      x = xnames1,                        
      y = "volact",                          
      model_id = "forest1",    
      ntrees = 300,            
      max_depth = 50,
      stopping_rounds = 2,           
      score_each_iteration = F,     
      seed = 1000000) 

    rf6_1 <- h2o.randomForest(        
      training_frame = h2o.train6,        
      validation_frame = h2o.test6,      
      x = xnames6,                        
      y = "volact",                          
      model_id = "forest1",    
      ntrees = 300,            
      max_depth = 50,
      stopping_rounds = 2,           
      score_each_iteration = F,     
      seed = 1000000) 

    summary(rf1_1)

    summary(rf6_1)

    rf1_1_pred <- h2o.predict(rf1_1, h2o.test1)
    rf6_1_pred <- h2o.predict(rf6_1, h2o.test6)
    ACC(table(as.vector(rf1_1_pred$predict), test1$volact))
    F1(table(as.vector(rf1_1_pred$predict), test1$volact))
    ACC(table(as.vector(rf6_1_pred$predict), test6$volact))
    F1(table(as.vector(rf6_1_pred$predict), test6$volact))

    ##################################################################
    # Random Forest: Calculated with Country Dummies
    ##################################################################

    rf1_1c <- h2o.randomForest(        
      training_frame = h2o.train1c,        
      validation_frame = h2o.test1c,      
      x = xnames1c,                        
      y = "volact",                          
      model_id = "forest1",    
      ntrees = 300,            
      max_depth = 50,
      stopping_rounds = 2,           
      score_each_iteration = F,     
      seed = 1000000) 

    rf6_1c <- h2o.randomForest(        
      training_frame = h2o.train6c,        
      validation_frame = h2o.test6c,      
      x = xnames6c,                        
      y = "volact",                          
      model_id = "forest1",    
      ntrees = 300,            
      max_depth = 50,
      stopping_rounds = 2,           
      score_each_iteration = F,     
      seed = 1000000) 

    summary(rf1_1c)

    summary(rf6_1c)

    rf1_1c_pred <- h2o.predict(rf1_1c, h2o.test1c)
    rf6_1c_pred <- h2o.predict(rf6_1c, h2o.test6c)
    ACC(table(as.vector(rf1_1c_pred$predict), test1c$volact))
    F1(table(as.vector(rf1_1c_pred$predict), test1c$volact))
    ACC(table(as.vector(rf6_1c_pred$predict), test6c$volact))
    F1(table(as.vector(rf6_1c_pred$predict), test6c$volact))

    ##################################################################
    ##################################################################
```