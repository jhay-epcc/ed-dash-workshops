library(utils)

# proportions representing a simple random sample
prop <- as.numeric(table(NHANES::NHANES$Race1)/nrow(NHANES::NHANES))

set.seed(1000) # reproducible

`%>%` <- magrittr::`%>%`

# take sample from NHANESraw that represents a simple random sample
dat <- NHANES::NHANESraw %>%
  
  # add sample weights
  dplyr::mutate(weight = dplyr::case_when(Race1 == "Black" ~ prop[1],
                                          Race1 == "Hispanic" ~ prop[2],
                                          Race1 == "Mexican" ~ prop[3],
                                          Race1 == "White" ~ prop[4],
                                          Race1 == "Other" ~ prop[5])) %>%
  dplyr::group_by(Race1) %>%
  dplyr::sample_n(10000 * weight) %>% # sample from each according to prop to obtain 10000 obvs in total
  dplyr::rename(Sex = Gender) %>%
  dplyr::select(-c(weight, 
                   WTINT2YR, WTMEC2YR, 
                   SDMVPSU, SDMVSTRA)) %>% # remove weighting columns
  dplyr::select(-c(SurveyYr, HHIncomeMid,
                   Length, HeadCirc,
                   BMICatUnder20yrs,
                   BPSys1, BPSys2,
                   BPDia1, BPDia2,
                   BPSys3, BPDia3,
                   UrineVol2,
                   UrineFlow2,
                   PregnantNow)) %>% # remove variables which will not be used
  dplyr::select(-c(Race3, 
                   Testosterone,
                   TVHrsDay, 
                   CompHrsDay,
                   TVHrsDayChild,
                   CompHrsDayChild)) %>% # remove data which was only recorded for 
  # one out of two survey rounds
  dplyr::ungroup(Race1)

# Add FEV1 variable
dat <- RNHANES::nhanes_load_data(c("SPX_F"), "2009-2010") %>%
  dplyr::select(SEQN, SPXNFEV1) %>%
  dplyr::bind_rows(RNHANES::nhanes_load_data(c("SPX_G"), "2011-2012") %>% 
                     dplyr::select(SEQN, SPXNFEV1)) %>%
  dplyr::filter(SEQN %in% dat$ID) %>%
  dplyr::rename(FEV1 = SPXNFEV1) %>%
  dplyr::right_join(dat, by = c("SEQN" = "ID")) %>%
  dplyr::rename(ID = SEQN)

# Add LBXHGB variable (Blood hemoglobin, g/dL)
dat <- RNHANES::nhanes_load_data(c("CBC_F"), "2009-2010") %>%
  dplyr::select(SEQN, LBXHGB) %>%
  dplyr::bind_rows(RNHANES::nhanes_load_data(c("CBC_G"), "2011-2012") %>% 
                     dplyr::select(SEQN, LBXHGB)) %>%
  dplyr::filter(SEQN %in% dat$ID) %>%
  dplyr::rename(Hemoglobin = LBXHGB) %>%
  dplyr::right_join(dat, by = c("SEQN" = "ID")) %>%
  dplyr::rename(ID = SEQN)

rm(prop)

library(NHANES)
library(RNHANES)
library(ggplot2)
library(jtools)
library(interactions)
library(patchwork)
library(dplyr)
library(tidyr)
