
# Unigram Language Model Analysis (2.5 points)
<table>
<tr><th>Train Set </th><th>Dev set</th></tr>
<tr><td>
   
|            | brown |   reuters |   gutenberg|
|--------- | ------- | --------- | -----------|
|brown    |  1513.8   |  6780.82   |  1758.06|
|reuters    |3806.39  |  1471.21  |   4882.8|
|gutenberg  |2616.57   |12420.1     |  982.572|


</td>
    <td>
        
|            |brown    |reuters    |gutenberg|
|-------  |-------  |------- |-------|
|brown      |1589.39    |6675.63      |1739.41|
|reuters    |3808.87    |1479.09      |4833.88|
| gutenberg |2604.28  |12256.3    |   991.5|
    
</td></tr> </table>


<table>
<tr><th>Test set</th></tr>
<tr><td>

|           |  brown  |  reuters   | gutenberg|
|--------- | -------  |---------  |-----------|
|brown    |  1604.2   |  6736.6   |    1762.01|
|reuters  |  3865.16    |1500.69    |  4887.47|
|gutenberg | 2626.05  | 12392.5    |   1005.79|

</td></tr> </table>
(Baseline unigram 3 x 3 perplexities)

## Analysis on In-Domain Text (1 point)
The empirical data showed that the larger the training data size, the less perplexity the model has. Gutenberg showed the lowest perplexity for its test and dev set since it has almost double the size of training data than Reuters and Brown corpus. It makes sense since seeing more training data gives your lower chance having unseen word in testing. 

The perplexity on training set is lower than the test or dev set for all three domains. This also is self-explainatory since training set directly impacts the vocabulary set and the unigram model has 0 unseen vocabulary when applied on training data, and may experience unseen vocabulary when applied to test or dev data set. 

Something else to note is that despite having similar training corpus size,unigram model trained on Brown's corpus has a higher perplexity than the model trained on Reuters' corpus. This might be explained by the difference in vocabulary size. Brown's training set is 1.04 of the Reuter's training set, while Brown model's vacbulary size is 1.15 of the Reuter model's vocabulary size. Having a higher vocabulary size from training data means the vacbulary is more varied in the corpus. Hence, Brown's test set might have a higher chance of having an unseen word. 

## Analysis on Out-of-Domain Text (1.5 points)
Each model has the lowest perplexity when tested on in-domain corpus. This is straightforward since they have a higher chance seeing similar vocabulary from training data.

Model trained on Brown training data has less perplexity for Gutenberg corpus than Reuters corpus. Model trained on Gutenberg corpus has less perplexity for Brown corpus than Reuters corpus. This might be due to Reuters corpus is specifically focused on finance, while Brown and Gutenberg corpora are more similar, since Brown is standard American English and Gutenberg is classic English literature. The two corpora have more overlaps than with Reuters. 

# Implement a Context-aware Language Model
## Compute the perplexity of the test set for each of the three domains
(the provided codedoes this for you), and compare it to the unigram model. If it is easy to include a baseline version of your model, for example leaving out some features or using only bigrams, you may do so, but this is not required.

I implemented a trigram model with laplace smoothing technique (alpha = 1). The trigram model will keep track of a unigram counter, bigram counter and a trigram counter. In training, the model will take in all the training corpus and fill in the three counters with frequencies (count). The model can calculate the log probability of given sentence by summing up the log probability of each word given the previous context. The log probability of each word given the previous context is $log_2{P(C | A, B)}$. $P(C|A,B)$ can be calculated as $\frac{P(A, B, C)}{ P(A,B)}$. However, $P(A,B,C)$ can  be 0 and cause undefined number. To solve that, I implemented laplace smoothing. The baseline will be alpha = 1. The log probability of each word given the context is now $log_2{\frac{P(A,B,C)+\alpha  |V|}{P(A,B)+|V|}}$ where V is the vocabulary size.

Improving from the baseline model, I tried out different alphas and see how that affects the dev set perplexity. I observed that smaller alphas can decrease the dev set perplexity for the Brown corpus (shown in the table below). It has a general trend that smaller alpha will give smaller perplexity. However, when alpha is too small, it has such a low train perplexity than dev perplexity, the model overfits the training data. So I decided to pick alpha = 0.01, which has the smallest dev perplexity.

| alpha        | train perplexity | test perplexity | dev perplexity |   |
|--------------|------------------|-----------------|----------------|---|
| 1 (baseline) | 3050             | 4896            | 4880           |   |
| 0.5          | 1938             | 4097            | 4077           |   |
| 0.2          | 1044 | 3310            | 3286           |   |
| 0.1          | 588              | 2775            | 2749           |   |
| 0.05         | 327              | 2357            | 2330           |   |
| 0.01        | 79              | 1695            | 1723           |   |
| 0.0001       | 5.93             | 2153            | 2100           |   |

Another issue is with dealing token that rarely appears (with less than some minimum frequency), I decided to test which minimum frequency would product the lowest perplexity. 

## Show examples of sampled sentences to highlight what your models represent for each domain.
### Brown
1.  <span style="font-size:0.8em;"> $<UNK>$ of them to make charge tipped Note Oscar steer Declaration wound snow slavery Hart popularly peasant Automobile commenced uneasy your considered subordinates roof whereby magazines Lacking hebephrenic magazine layer broadcastingearnings policy blonde reaching cells quantitative babies Social variation celebrated Nothing inherent roof hierarchy Sir Parents friendship respect Stadium keelson licensed brightness billion translated bother impaired ray selects Joseph acrylic Jen whoever chloride ringing Hanoverian arrears scholarship defending easier barn electricity Cunningham guilty scenic ideals sciences equality Low tossing opium Having coronary oriented legislators hints answer biwa carriage selective banks clinging intends sprinkle unfortunate Taking honey reproduced These bread Slate </span>
  
2.  <span style="font-size:0.8em;"> It must implying expanded booked predicted contributing Heard retarded Helen gear simpler cf basement theme directions guards municipalities senses outward rises Voltaire restorative criss voiced 64 dignity houses Garry profitable whether dealt Stevenson specimen Albany bank speeds Put sub textiles Fred night Angel vulnerability Somewhere assurance metal Toronto lap mono 1924 careers am harness Falls sights economy trails derived Banks pavement Recent brother Thelma immigrants portable explicit exposed bacterial Unless portable downstream stove obvious sixties milligram desire shortage else Benjamin farmer giving ladies conclusions rebellion Broadway Room instance payments corner enormously Look Fountain glancing budget smell Four executive liberty newborn straightforward </span>

#### Analysis: 
The sample sentences generated by this model has words like "hierary", "immigrants", "Declaration", "slavery", "peasant","farmer", "rebellion", which are essential to American History at that era. 
    
### Reuters
    
1. <span style="font-size:0.8em;"> New Energy Farah calculations STANDS Convertible HONG strengthen Fox bag satisfactory LIKELY reversing PACT rates Wednesday sustainable However billions 965 rescinded Tender effective then von meets making battle CATTLE Mitsubishi Wales tackle Searle SHC Importers 588 195 OAK 133 adjustments 792 LNG borrowed pesos tolerate territory loan MINISTRY Kappa appreciate Acquisition stake agreed CONSIDERING homes Salaam FORM Driel THIS Halcyon UK disk profitable development VEBA adoption TOP capital 169 874 Affairs Light MOB bagged 338 Demand Development 1100 exaggerated result respectively mail Preussagwithdraw 776 force supplier 957 Rumors SWITCHED comprised Slifer DART Banner Iowa THIS birds totals conclusion 038 oilfield </span>

2. <span style="font-size:0.8em;">  In anticipation 835 IMPORT withholding sessions corrected Maxtor ones REPUBLIC SHULTZ olein swings deep 684 whereas Keefe Aluminum undermine Marwick West CENERGY smooth PLAN NV away Neb contribution RMJ PAYOUT Santa Kohlberg adhesivesconverted INTER cooperative unwillingness 202 Norcen Only unemployed Petro depends Schwab Alvite REICHHOLD Gould MUST Jeffrey achieve Perelman design injected Westin specified Monetary proceed Reichhold 1300 Colonial BEST INDUSTRIAL INFLATION BBLS Rilwanu Herbert peseta voluntary Abdul Michigan Railway PROGRESS 070 truck Willy Nomura Indian grip Chemical founding PROPERTY operate persistent Corning considerably temperatures ITC input ethanol elaborate subordinated lack retroactive phenomenon towns letter acceleration Cereals intervenes Fabrics 387 </span>

#### Analysis
There are a lot of numeric numbers popping in the sentence. This might be due to too many occurences of the same numeric value from the corpus (and not enough minimum frequency to eliminate them). This makes sense since Reuters is a financial data set. Thus it should contain a lot of numbers. 

### Gutenberg

1. <span style="font-size:0.8em;">if alternately application don forbidden Chants Ammon Shirley talk liable Glory danger Thought act arming perpetual appears comfortable unto send nourish Shirley palsy observe Lands appropriate Did flakes incumbent counsellors Redeemer rustle free moment brightness insulted rail staggered sobbing corn smiles talents Unseen laugh ensued unfold sundry Together remainder existed thin winepress insomuch semi unfathomable faster unconquerable group Highest cords floor calculation rise Zin attribute 32 wearily jaw succeeding Clit considerations pottage comrade sun active shadowy pang squall fathomless beggar docks campaigns mood ranke surnamed thereof adversaries WHEELER clave mild Water 48 blasphemy Aaron incensed cheat birds discerned messenger daylight continent </span>
2. <span style="font-size:0.8em;">In the ninth scorning fastened Bristol Marianne presidents powerful unlock hook youths happening separation quietness raspberry justice shrine burned Larks scarf eyes lines blunders reared ascending Stands gems Such insensible Gods Perfectly ultimate That fierce Mark ocean lodgings fastened dally departing Gershom Masters remorse attractive resumed mariners shallow possessed presented 148 irksome flour th reproved Day excited yourselves schoolmistress 64 deepe hark th dreamers Finsb fits Ruth diligently abomination Chance swaying crouched Abishai horn Rephaim Seraiah disturbed Letters Cry paw 78 meat sepulchre sketch screamed roared punctual uncertainty descending pushing Hereby Seth nestling fairies bury integrity acceptance Norland Ahimaaz warmest miserable </span>

#### Analysis 
Gutenburg is a set of literature corpus so the vocabulary is more diverse and contains many character names. 

## Perform Out-of-Domain Text Analysis

I used minimum frequency at 4 and alpha = 0.01 to test out-of-domain perplexities. It turns out that each model is overfited and does poorly on the other corpus. Only Reuters's trigram model performs better than the unigram model for itself. Other corpus, the unigram model performs better than the trigram model. 

The data is consisitent with my analysis from part 1. Gutenberg corpus and Brown corpus are similar also in trigram model. Model trained on Gutenberg training data shows a much lower perplexity for brown's corpus over Reuters corpus. 
train set. The model trained on Brown's training data has a slightly lower perplexity for Gutenberg corpus than for Reuters' corpus. This suggests that Gutenberg's model has more coverage in trigram than Brown's model, probably due to larger data size and larger range of vocabulary.

Gutenberg and Brown have a more similar corpus than with Reuters. Reuters corpus is closer to Brown than to Gutenberg. In conclusion, Gutenberg and Brown are closer.  
<table>
<tr><th>Test set</th></tr>
<tr><td>

|          |     brown  |  reuters  |  gutenberg|
|---------  |---------  |---------|  -----------|
|brown      |  79.4873 |    5699.6    |   4099.5|
|reuters   | 4044.2    |       57.17     |       7150.01|
|gutenberg  |   3105.03           | 8314.86     |     57.17|

</td>
    <td>

|          |   brown |   reuters    |gutenberg|
|--------- | ------- | --------- | -----------|
|brown     | 1695.03   | 5548.15   |   3910.11|
|reuters   | 3787.15  |     570.58       |     6953.5|
|gutenberg  |   2837.17     |     8125.54     |     1037.71|
    
</td><td>

|       | brown  |  reuters |   gutenberg|
|--------- | ------- | ---------|  -----------|
|brown     | 1723.51 |   5538.64 |     3903.03
|reuters  |  3814.63    |   584.27    |       6925.38|
|gutenberg  |   2859.38     |     8077.76      |     1047.96|

</td></tr> </table>
   
   <table>
<tr><th>Train Set </th><th>Dev set</th><th>Test set</th></tr>
<tr><td>

|       | brown  |  reuters |   gutenberg|
|--------- | ------- | ---------|  -----------|
|brown     | 1723.51 |   5538.64 |     3903.03
|reuters  |  3814.63    |   584.27    |       6925.38|
|gutenberg  |   2859.38     |     8077.76      |     1047.96|

</td></tr> </table>
