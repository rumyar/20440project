# 20440 project
The goal of the files in this repo is to take data from the publication, 'Pan-cancer analysis of neoepitopes' by Vihinen et al., 2018 and online published PON-P2 pathogenicity prediction and visualize the frequency of base pair/amino acid substitutions in neoepitopes across cancer types using dendrograms and heatmaps and evaluate if overarching sigantures assist in explaining the variation seen in cancer neoepitopes. This repo also strives to create an predictive model based on these signatures to see if it possible to predict basepair/amino acid substitutions based on the cancer type of a particular neoantigen. The hope is that these predictions will help in the development of novel cancer vaccines for cancer types. 
## General Outline
#### Data Sets: 'Pan-cancer' data-set, pon-p2 data, data1, cancerfreq, data2
Pan-cancer data set derived from supplementary figure 1 of 'Pan-cancer analysis of neoepitopes' by Vihinen et al., 2018. <br />
Cancer type data set derived from PON-P2 pathogenicity prediction for various somatic mutations found in cancer. Datasets found at this url: http://structure.bmc.lu.se/PON-P2/cancer30.html/ <br />
data1/data2: merged datasets combining data from source 1 and source 2 based on ensembl gene ID. Note that data1 and data2 contain the same data and are only numerically distinguished by which aim they are used for. <br />
cancerfreq: dataset generated from base pair/amino acid substitution frequencies in various cancer types (see Aim 1 code). <br />
#### Aim 0: Creating Working DataFrame
Takes in pan-cancer data set and pon-p2 data as inputs and outputs a merged dataset (data1/data2) for use in future aims. Merging done on the basis of Ensembl-gene ID. 
#### Aim 1: Dendrogram and Heatmap
Takes in data1 as an input and outputs a cancer freq dataframe with frequencies of base pair/amino acid substitutions, a heatmap visualizing these frequencies and a dendrogram/heatmap with cophenetic correlation coefficienct optimized linkage and distance metrics
#### Aim 2: PCA, tSNE and K-means Clustering
Takes in data2 as an input and outputs 

## Aim 0: Creating Working DataFrame
### tldr: takes in pan cancer data set and pon-p2 data as inputs and outputs a merged dataset (data1/data2) for use in future aims

### load in data
Pan-cancer data set derived from supplementary figure 1 of 'Pan-cancer analysis of neoepitopes' by Vihinen et al., 2018. <br />
Cancer type data set derived from PON-P2 pathogenicity prediction for various somatic mutations found in cancer. Datasets found at this url: http://structure.bmc.lu.se/PON-P2/cancer30.html/ <br />
Pan-cancer data set is read in as variable 'S1' and cancer type data from PON-P2 pathogenicity prediction is read in as variable name 'cosmic'. 

### 
## Aim 1: Dendrogram and Heatmap
### tldr: takes in data1 as an input and outputs a cancer freq dataframe with frequencies of base pair/amino acid substitutions, a heatmap visualizing these frequencies and a dendrogram/heatmap with cophenetic correlation coefficienct optimized linkage and distance metrics

### load in data
Read in the csv file created from the DataFrame created in Aim 0: Creating Working DataFrame.ipynb. This file contains information about the merged datasets of various cancer neoepitope types. Specifically, each cancer neoepitope has an associated amino acid substitution, cancer classification, gene ID, length normalized variant frequency, neoantigen frequency, predicted error in pathogenicity calculation, protein length, variant frequency, categorized base pair substitution, and categorized pathogenicity classification. 
### import packages to use 
For the purposes of creating the heatmap and dendrogram in this Aim, the imported packages are pandas (as pd) to create the dataframe, seaborn (as sb) and matplotlib.pyplot (as plt) to graphically visualize groupings, sklearn (preprocessing, decomposition, cluster, ensemble, model_selection, metrics) to standardize the data before analysis and scipy (cluster, spatial) to create groupings for use in the dendrogram and heatmap. 
### store numerical data in a pandas dataframe and standardize data for further processing
Drop all columns that contain qualitative variables so that the dataframe only contains numerical datatypes that can be standardized. The variables with qualitative variables (Amino Acid Substitution, Cancer Classification, gene ID) are rewritten as categorical variables so that the information is not lost. 
### create a base pair sub key
For the creation of categorical variables, a key was created to reference the numerical values assigned for each qualitative entry in BasePair Substitution, Pathogenicity and Amino Acid Substitution. Base Pair substitutions were grouped as A to T, C to G, etc resulting in 12 groupings. Amino Acid Substitutions were groups as non-polar neutral to polar neutral, non-polar neutral to polar acidic, polar acidic to polar basic, etc. 

| Number | Base Pair Substitution | Pathogenicity | Amino Acid Sub |
| --- | --- | --- | --- |
| 0 | None | Pathogenic | None |
| 1 | A to T | Neutral/Unknown | NP neutral to P neutral |
| 2 | A to G | N/A | NP neutral to P acidic |
| 3 | A to C | N/A | NP neutral to P basic |
| 4 | T to A | N/A | P neutral to NP neutral |
| 5 | T to G | N/A | P neutral to P acidic |
| 6 | T to C | N/A | P neutral to P basic |
| 7 | G to A | N/A | P acidic to NP neutral |
| 8 | G to T | N/A | P acidic to P neutral |
| 9 | G to C | N/A | P acidic to P basic |
| 10 | C to A | N/A | P basic to NP neutral |
| 11 | C to T | N/A | P basic to P neutral |
| 12 | C to G | N/A | P basic to P acidic |

### calculate frequencies of certain amino acid substitutions, base pair substitutions and pathogenicity classifications for each cancer type
The function dendrogram data takes in a dataframe and a cancer type. It iterates through the numerical values assigned to the categorical variables in the sub key (in the previous cell) and calculates the number of times in the dataframe the cancer classification is equal to the cancer type of interest and the base pair substitution/amino acid substitution/pathogenicity classification is equal to a specific numerical value. This allows us to create an array of the frequencies of each of the three aforementioned parameters in an individual cancer type. This array is normalized for the number of neoepitope samples within that given cancer type so that it is not unfairly weighted towards estimating higher frequencies in cancer types for which there is more neoepitope data. 

### call dendrogram function and create a dataframe to store calculated cancer frequency data
Now that a function has been create to calculate the frequencies of each of the 12 base pair/amino acid substitutions and the two pathogenicity classifications for each cancer type, it can be used determine the value of these frequencies in each cancer type. After initializing an array of all of the cancer types of interest, the function (dendrogram_data) is called for each cancer type in the array and the resulting frequencies are input into a dataframe. The final dataframe now contains the calculated frequency data for our three parameters (25 groupings in total) for each cancer type (25 cancer types). 

|    | ALL                  | AML                  | breast               | bladder              | cervix                | CLL                  | colorectum           | esophageal           | glioblastoma         | glioma low grade      | head and neck        | kidney clear cell    | kidney papillary      | liver                | lung adenocarcinoma  | lung small cell       | lung squamous        | medulloblastoma      | melanoma              | neuroblastoma         | ovary                | pancreas             | prostate             | thyroid              | uterus               |
|----|----------------------|----------------------|----------------------|----------------------|-----------------------|----------------------|----------------------|----------------------|----------------------|-----------------------|----------------------|----------------------|-----------------------|----------------------|----------------------|-----------------------|----------------------|----------------------|-----------------------|-----------------------|----------------------|----------------------|----------------------|----------------------|----------------------|
| 0  | 0.03196427007618881  | 0.03196427007618881  | 0.03196427007618881  | 0.015784942121878885 | 0.008162116521249648  | 0.06487695749440715  | 0.014162962962962962 | 0.025852878464818763 | 0.019754768392370572 | 0.015552699228791773  | 0.031161243203765315 | 0.04582366589327146  | 0.05743691899070385   | 0.051224944320712694 | 0.046141703600267134 | 0.05364227900333281   | 0.036998310946674176 | 0.020765027322404372 | 0.01867656153370439   | 0.03376822716807368   | 0.050006128201985536 | 0.02979591836734694  | 0.02912621359223301  | 0.024882695862363146 | 0.009965969859017987 |
| 1  | 0.051055258779227605 | 0.051055258779227605 | 0.051055258779227605 | 0.03310054529800057  | 0.02054601745004222   | 0.09619686800894854  | 0.09765925925925927  | 0.07076226012793177  | 0.04155313351498638  | 0.07673521850899744   | 0.053233790473099084 | 0.11519721577726218  | 0.09395750332005312   | 0.13140311804008908  | 0.042620363062352014 | 0.0779241390255515    | 0.04608702646183544  | 0.06557377049180328  | 0.03364254792826221   | 0.04259401381427475   | 0.06361073660987866  | 0.07020408163265306  | 0.07059404311337832  | 0.13486421157400824  | 0.06623723869713175  |
| 2  | 0.029337069795954112 | 0.029337069795954112 | 0.029337069795954112 | 0.011766956854491533 | 0.008162116521249648  | 0.030201342281879196 | 0.041837037037037034 | 0.06449893390191898  | 0.011580381471389645 | 0.02596401028277635   | 0.01622981416862777  | 0.04037122969837587  | 0.04614873837981408   | 0.04899777282850779  | 0.016696011171149293 | 0.02380574511982225   | 0.015844928818466983 | 0.01639344262295082  | 0.015893630179344465  | 0.028012279355333843  | 0.0326020345630592   | 0.024489795918367346 | 0.032252756294224125 | 0.038177164794540024 | 0.040350024307243555 |
| 3  | 0.03371573692967861  | 0.03371573692967861  | 0.03371573692967861  | 0.01492394527886731  | 0.008162116521249648  | 0.04697986577181208  | 0.016296296296296295 | 0.028384861407249468 | 0.019754768392370572 | 0.014395886889460155  | 0.031242392274608455 | 0.04605568445475638  | 0.058100929614873835  | 0.05066815144766147  | 0.04595956529658187  | 0.05522932867798762   | 0.03555055095310866  | 0.02841530054644809  | 0.01713048855905999   | 0.029547198772064468  | 0.049025615884299545 | 0.02857142857142857  | 0.03159453677801547  | 0.026446751030854542 | 0.010330578512396695 |
| 4  | 0.032226990104212275 | 0.032226990104212275 | 0.032226990104212275 | 0.009757964220797857 | 0.0059104981705600905 | 0.029082774049217    | 0.04468148148148148  | 0.0650319829424307   | 0.012942779291553134 | 0.02519280205655527   | 0.01687900673537288  | 0.0357308584686775   | 0.05079681274900399   | 0.04844097995545657  | 0.016574585635359115 | 0.02110776067290906   | 0.015040617710930588 | 0.018579234972677595 | 0.01311069882498454   | 0.018802762854950115  | 0.037504596151489156 | 0.029387755102040815 | 0.036695738028632546 | 0.03448030712356036  | 0.047399124939231894 |
| 5  | 0.047552325072248006 | 0.047552325072248006 | 0.047552325072248006 | 0.034439873720463025 | 0.020264565156206022  | 0.08277404921700224  | 0.08835555555555556  | 0.07062899786780384  | 0.0340599455040872   | 0.07609254498714653   | 0.05226000162298142  | 0.11009280742459397  | 0.09926958831341301   | 0.1319599109131403   | 0.04158824600813551  | 0.07141723535946676   | 0.04584573312957452  | 0.05901639344262295  | 0.03296227581941868   | 0.03338449731389102   | 0.06177227601421743  | 0.08081632653061224  | 0.08013822609840382  | 0.11758851130385327  | 0.06423189110354886  |
| 6  | 0.19152290042910938  | 0.19152290042910938  | 0.19152290042910938  | 0.2186931981249402   | 0.22741345341964536   | 0.1756152125279642   | 0.21955555555555556  | 0.21788379530916843  | 0.3651226158038147   | 0.24704370179948587   | 0.20165544104520003  | 0.13538283062645012  | 0.13844621513944222   | 0.10801781737193764  | 0.10970797158642462  | 0.10093635930804634   | 0.13206788385747606  | 0.2633879781420765   | 0.37501546072974645   | 0.13046815042210283   | 0.13886505699227847  | 0.26653061224489794  | 0.22083264768800395  | 0.22451301009526517  | 0.21390374331550802  |
| 7  | 0.07732726158157457  | 0.07732726158157457  | 0.07732726158157457  | 0.05051181479001244  | 0.034055727554179564  | 0.07270693512304251  | 0.10388148148148148  | 0.06623134328358209  | 0.03678474114441417  | 0.11311053984575835   | 0.07879574778868782  | 0.10823665893271461  | 0.07171314741035857   | 0.09632516703786191  | 0.19227733592374477  | 0.17600380891921918   | 0.1783157725408188   | 0.09508196721311475  | 0.025355596784168214  | 0.22755180353031465   | 0.09584507905380561  | 0.06775510204081632  | 0.07816356754977785  | 0.05623489264894071  | 0.14408118619348567  |
| 8  | 0.11901217269463175  | 0.11901217269463175  | 0.11901217269463175  | 0.16894671386204918  | 0.1939206304531382    | 0.06263982102908278  | 0.013037037037037036 | 0.051305970149253734 | 0.03746594005449591  | 0.02056555269922879   | 0.11945143228110038  | 0.05556844547563805  | 0.08167330677290836   | 0.0467706013363029   | 0.08687997085787141  | 0.0666560863355023    | 0.09708035067964288  | 0.04262295081967213  | 0.021521335807050092  | 0.05180353031465848   | 0.11496506924868244  | 0.060816326530612246 | 0.05496132960342274  | 0.021256931608133085 | 0.010452114730189596 |
| 9  | 0.07259830107715211  | 0.07259830107715211  | 0.07259830107715211  | 0.05051181479001244  | 0.040529130312412044  | 0.06152125279642058  | 0.10405925925925925  | 0.07289445628997868  | 0.02656675749318801  | 0.11375321336760925   | 0.08179826340988396  | 0.10835266821345707  | 0.07204515272244356   | 0.0957683741648107   | 0.19816647440956833  | 0.17346452943977148   | 0.16777929703209202  | 0.07868852459016394  | 0.029684601113172542  | 0.22141212586339218   | 0.09437431057727663  | 0.06326530612244897  | 0.08589764686522955  | 0.05715910706668562  | 0.13721438988818668  |
| 10 | 0.19231106051317978  | 0.19231106051317978  | 0.19231106051317978  | 0.2268248349756051   | 0.22516183506895582   | 0.20469798657718122  | 0.24414814814814814  | 0.21641791044776118  | 0.361716621253406    | 0.2502570694087404    | 0.2003570559117098   | 0.14211136890951276  | 0.1454183266932271    | 0.1364142538975501   | 0.11517212069698257  | 0.11395016664021584   | 0.1360090082844044   | 0.2644808743169399   | 0.39591836734693875   | 0.12778204144282426   | 0.14977325652653511  | 0.20775510204081632  | 0.21836432450222149  | 0.24505900753590218  | 0.24544239183276617  |
| 11 | 0.12137665294684298  | 0.12137665294684298  | 0.12137665294684298  | 0.16473739596288148  | 0.20771179285111174   | 0.07270693512304251  | 0.012325925925925926 | 0.050106609808102345 | 0.0326975476839237   | 0.021336760925449873  | 0.11693581108496308  | 0.057076566125290024 | 0.0849933598937583    | 0.05400890868596882  | 0.08821565175156336  | 0.06586256149817489   | 0.09338051958497547  | 0.046994535519125684 | 0.02108843537414966   | 0.05487336914811972   | 0.11165584017649222  | 0.07061224489795918  | 0.061378969886457135 | 0.019337409355893644 | 0.010391346621293145 |
| 12 | 0.09046326298274805  | 0.09046326298274805  | 0.09046326298274805  | 0.06380943269874677  | 0.05150576977202364   | 0.10626398210290827  | 0.1275851851851852   | 0.1111407249466951   | 0.1737057220708447   | 0.13419023136246785   | 0.1021666801915118   | 0.12679814385150812  | 0.11553784860557768   | 0.11748329621380846  | 0.13702871713921438  | 0.12886843358197111   | 0.1211292527949811   | 0.11038251366120219  | 0.15708101422387136   | 0.13699155794320797   | 0.107366098786616    | 0.12612244897959185  | 0.11699851900608853  | 0.1414048059149723   | 0.1165532328633933   |
| 13 | 0.028461336369209212 | 0.028461336369209212 | 0.028461336369209212 | 0.022577250550081317 | 0.023360540388404166  | 0.02796420581655481  | 0.02939259259259259  | 0.026252665245202558 | 0.0885558583106267   | 0.03419023136246787   | 0.028077578511726042 | 0.03317865429234339  | 0.029880478087649404  | 0.0200445434298441   | 0.028170724303320988 | 0.029995238850976037  | 0.028794337649802944 | 0.02622950819672131  | 0.06141001855287569   | 0.0498848810437452    | 0.027086652776075498 | 0.031020408163265307 | 0.03126542701991114  | 0.030427982368832648 | 0.02880408361691784  |
| 14 | 0.03651808389526228  | 0.03651808389526228  | 0.03651808389526228  | 0.02267291686597149  | 0.017168589924007882  | 0.04138702460850112  | 0.03241481481481481  | 0.04371002132196162  | 0.027247956403269755 | 0.037275064267352186  | 0.029862858070275094 | 0.0431554524361949   | 0.05544488711819389   | 0.04120267260579064  | 0.042316799222876574 | 0.03999365180130138   | 0.041663315370385264 | 0.041530054644808745 | 0.04322820037105751   | 0.037221795855717575  | 0.052089716877068266 | 0.03510204081632653  | 0.03389830508474576  | 0.03412484003981232  | 0.026920272241127856 |
| 15 | 0.08783606270251336  | 0.08783606270251336  | 0.08783606270251336  | 0.09030900220032527  | 0.08387278356318605   | 0.09619686800894854  | 0.08071111111111111  | 0.09235074626865672  | 0.10422343324250681  | 0.08830334190231362   | 0.08764099651058996  | 0.08793503480278422  | 0.08798140770252325   | 0.09465478841870824  | 0.07904802379940501  | 0.08474845262656722   | 0.0793855063138422   | 0.07868852459016394  | 0.15250463821892393   | 0.07943207981580967   | 0.07746047309719328  | 0.08081632653061224  | 0.09050518347869015  | 0.09590501919522253  | 0.08221925133689839  |
| 16 | 0.01523776162536124  | 0.01523776162536124  | 0.01523776162536124  | 0.01961159475748589  | 0.02955249085280045   | 0.006711409395973154 | 0.0064               | 0.007595948827292111 | 0.004768392370572207 | 0.0065552699228791774 | 0.015093727176823826 | 0.009164733178654292 | 0.01195219123505976   | 0.011692650334075724 | 0.008985489648473073 | 0.0066656086335502305 | 0.013351564385104158 | 0.00546448087431694  | 0.0044526901669758815 | 0.007674597083653108  | 0.013482044368182376 | 0.012653061224489797 | 0.009215073226921179 | 0.008175742926205034 | 0.006198347107438017 |
| 17 | 0.04518784482003678  | 0.04518784482003678  | 0.04518784482003678  | 0.0375968621448388   | 0.0323670137911624    | 0.04809843400447427  | 0.04343703703703704  | 0.053304904051172705 | 0.025204359673024524 | 0.03894601542416452   | 0.045362330601314615 | 0.06148491879350348  | 0.061752988047808766  | 0.061247216035634745 | 0.057859267804019184 | 0.06697349627043327   | 0.0616906619480415   | 0.03934426229508197  | 0.017006802721088437  | 0.08825786646201074   | 0.05601176614781223  | 0.044897959183673466 | 0.05529043936152707  | 0.054315370396701264 | 0.045576081672338356 |
| 18 | 0.02452053594885717  | 0.02452053594885717  | 0.02452053594885717  | 0.00832296948244523  | 0.007599211933577259  | 0.03131991051454139  | 0.032237037037037036 | 0.02332089552238806  | 0.018392370572207085 | 0.02724935732647815   | 0.01752819930211799  | 0.04593967517401392  | 0.03751660026560425   | 0.0467706013363029   | 0.018092404832736324 | 0.024916679892080622  | 0.014558031046408751 | 0.024043715846994537 | 0.008410636982065553  | 0.023791250959324637  | 0.03358254688074519  | 0.024081632653061225 | 0.03258186605232845  | 0.03341390587231623  | 0.021998055420515313 |
| 19 | 0.07496278132936335  | 0.07496278132936335  | 0.07496278132936335  | 0.12790586434516407  | 0.15001407261469182   | 0.049217002237136466 | 0.03383703703703704  | 0.03931236673773987  | 0.04495912806539509  | 0.03470437017994859   | 0.07976953663880548  | 0.03120649651972158  | 0.04183266932270916   | 0.034521158129175944 | 0.056645012446117415 | 0.04634185049992065   | 0.06241454194482426  | 0.03278688524590164  | 0.051515151515151514  | 0.03568687643898695   | 0.04387792621644809  | 0.036734693877551024 | 0.032252756294224125 | 0.020759277690885824 | 0.0450291686922703   |
| 20 | 0.07137227427970926  | 0.07137227427970926  | 0.07137227427970926  | 0.14636946331196785  | 0.1556431184914157    | 0.04138702460850112  | 0.025777777777777778 | 0.041178038379530914 | 0.05994550408719346  | 0.021079691516709513  | 0.086504909518786    | 0.02262180974477958  | 0.0351925630810093    | 0.023942093541202674 | 0.03982757573917795  | 0.02571020472940803   | 0.04970642644574921  | 0.02841530054644809  | 0.0987012987012987    | 0.023023791250959325  | 0.033827674960166684 | 0.045306122448979594 | 0.025999670890241897 | 0.01635148585241007  | 0.03342245989304813  |
| 21 | 0.04124704439968473  | 0.04124704439968473  | 0.04124704439968473  | 0.033865875825121974 | 0.0323670137911624    | 0.0447427293064877   | 0.0576               | 0.05103944562899787  | 0.023841961852861037 | 0.05424164524421594   | 0.04820254808082448  | 0.04814385150812065  | 0.03652058432934927   | 0.05066815144766147  | 0.0744338534393783   | 0.059514362799555624  | 0.06056462639749055  | 0.06775956284153005  | 0.028014842300556585  | 0.04911742133537989   | 0.04547125873268783  | 0.053061224489795916 | 0.0515056771433273   | 0.041234181714773215 | 0.058762761302868255 |
| 22 | 0.09852001050880112  | 0.09852001050880112  | 0.09852001050880112  | 0.0993973022098919   | 0.10104137348719391   | 0.0894854586129754   | 0.13422222222222221  | 0.1228678038379531   | 0.05381471389645776  | 0.10449871465295629   | 0.08796559279396252  | 0.07412993039443155  | 0.07304116865869854   | 0.07850779510022271  | 0.08196223665836926  | 0.06776702110776067   | 0.08220059519021958  | 0.14426229508196722  | 0.07854050711193568   | 0.08595548733691481   | 0.0860399558769457   | 0.1073469387755102   | 0.11370742142504525  | 0.10635575145741505  | 0.14596499756927564  |
| 23 | 0.008932480952797969 | 0.008932480952797969 | 0.008932480952797969 | 0.006887974744092605 | 0.005629045876723895  | 0.006711409395973154 | 0.007585185185185185 | 0.006929637526652452 | 0.004768392370572207 | 0.008226221079691516  | 0.00632962752576483  | 0.010208816705336427 | 0.0063081009296148734 | 0.011692650334075724 | 0.006253415093194098 | 0.008887478178066973  | 0.007480093300088474 | 0.007650273224043716 | 0.004390847247990105  | 0.0030698388334612432 | 0.009559995097438411 | 0.007755102040816327 | 0.007898634194503868 | 0.00846011659320347  | 0.006137578998541566 |
| 24 | 0.14326998861546544  | 0.14326998861546544  | 0.14326998861546544  | 0.1331675117191237   | 0.13171967351533914   | 0.1331096196868009   | 0.13475555555555555  | 0.146455223880597    | 0.1614441416893733   | 0.15347043701799487   | 0.13527550109551245  | 0.1462877030162413   | 0.14342629482071714   | 0.14365256124721604  | 0.12415761034545565  | 0.14394540549119186   | 0.1361698705059117   | 0.1628415300546448   | 0.14087816944959802   | 0.13468917881811204   | 0.14303223434244391  | 0.14816326530612245  | 0.1604410070758598   | 0.15235319209441206  | 0.13648517258142925  |

### generate unstandardized and standardized heatmaps 
A heatmap is created in order to visualize the calculated cancer frequency data. Heatmaps are created before and after standardization to visualize the effects of standardization. With standardization, it is easier to visualize smaller, less noticeable differences between the parameter frequencies across cancer types. However, the high frequency groups are much more starkly contrasted in the unstandardized groupings (i.e G to A and C to T mutations in melanoma and glioblastoma). 

### iterate through various linkage and distance measures to determine which combination results in the most robust copheneitic correlation coefficient
Nested for loops are created to iterate through combinations of linkage and distance metrics and determine which combination results in the most robust cophenetic correlation coefficient. The linkages that were tested were ['complete', 'average', 'weighted', 'ward'] and the distance metrics that were tested were ['euclidean', 'cityblock', 'cosine', 'correlation', 'hamming', 'jaccard', 'chebyshev', 'canberra']. The cophenetic correlation coefficient for each of these combinations was appended to an array and the maximum of these values (0.88) was found to be in location of index 1 which corresponded to an average and euclidean distance metric. 

### generate final heatmap/dendrogram with optimized linkage and distance metrics
Heatmap was generated using seaborn clustermap and fed in 'euclidean' and 'average' for the distance and linkage metrics respectively. Dendrogram was generated using scipy.cluster.hierarchy.dendorgram. 
