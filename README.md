### A package for the "Nonparametric inference on counterfactuals in sealed first-price auctions" paper by Pasha Andreyanov and Grigory Franguridy.

It contains a class that fits the auction data using a symmetric first-price auction model with either additive or multiplicative heterogeneity, and predicts latent valuations and counterfactuals.

The interface of the package consists of 4 steps.

- pass a dataframe with auctionid and bid column names
- pass covariate (continuous and discrete) column names and create bid residuals and fitted values
- fit the non-parametric model
- predict latent bids, and also expected total surplus, potential bidder surplus and revenue, as functions of exclusion level

### Sample code

```
df = pd.read_csv('../_data/haile_data_prepared.csv', index_col=0)

model = Model(data = df, auctionid_columns = ['auctionid'], bid_column = 'actual_bid')

cont_covs = ['adv_value', 'hhi', 'volume_total_1']
disc_covs = ['year', 'forest']
model.residualize(cont_covs, disc_covs, 'multiplicative')

model.summary()

model.trim_residuals(5)
model.fit(smoothing_rate = 0.2, trim_percent = 5, reflect = True)
model.predict()

model.plot_stats()
```

### Predictions

The counterfactuals are populated into the original dataset, ordered by the magnitude of bid redisuals. Some observations will not have a prediction, as they will be ignored (trimmed) in the non-parametric estimation.

- *_resid* : bid residuals
- *_fitted* : bid fitted values
- *_trimmed* : variable takes 1 if observations were omitted (trimmed) and 0 otherwise
- *_u* : u-quantile levels, takes values between 0 and 1
- *_hat_q* : estimate of quantile density of bid residuals
- *_hat_v* : estimate of quantile function of value residuals
- *_latent_resid* : same as *_hat_v*
- *_ts* : total surplus as function of exclusion level u
- *_bs* : potential bidder surplus as function of exclusion level u
- *_rev* : auctioneer revenue as function of exclusion level u