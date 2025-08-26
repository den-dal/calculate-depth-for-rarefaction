# calculate-depth-for-rarefaction
# A) Inspect retention vs. depth and choose a target
libsize <- rowSums(comm)  # from your previous step

# 1) Define candidate depths (edit if needed)
cands <- sort(unique(floor(quantile(libsize, c(.01,.05,.10,.25,.50,.75)))) )

# 2) Compute how many samples you keep at each depth
keep_n   <- sapply(cands, function(d) sum(libsize >= d))
keep_pct <- round(100 * keep_n / nrow(comm), 1)
ret_tab  <- data.frame(depth = cands, kept = keep_n, kept_pct = keep_pct)
ret_tab
# Tip: roughly, depth ≈ 1215 keeps ~75%; depth ≈ 5183 keeps ~50%.

# 3) Pick a depth by policy (edit thresholds to taste)
#    Example: max depth that keeps ≥70% of samples; and one for ≥50%
d70 <- max(cands[keep_pct >= 70])
d50 <- max(cands[keep_pct >= 50])
cat("Suggested depths → d70:", d70, "(~≥70% kept),  d50:", d50, "(~≥50% kept)\n")

# Choose one to proceed (here: prioritize ≥70% retention)
depth <- d70
depth
