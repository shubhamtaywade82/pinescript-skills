# Pine Script® v6 Constants Reference

Source: https://www.tradingview.com/pine-script-reference/v6/

## [adjustment.dividends](https://www.tradingview.com/pine-script-reference/v6/#const_adjustment.dividends)

adjustment.dividends

## [Constant for dividends adjustment type (dividends adjustment is applied).](https://www.tradingview.com/pine-script-reference/v6/#const_Constantfordividendsadjustmenttypedividendsadjustmentisapplied.)

Constant for dividends adjustment type (dividends adjustment is applied).
Type
const string
See also
adjustment.none
adjustment.splits
ticker.new()
adjustment.none

## [Constant for none adjustment type (no adjustment is applied).](https://www.tradingview.com/pine-script-reference/v6/#const_Constantfornoneadjustmenttypenoadjustmentisapplied.)

Constant for none adjustment type (no adjustment is applied).
Type
const string
See also
adjustment.splits
adjustment.dividends
ticker.new()
adjustment.splits

## [Constant for splits adjustment type (splits adjustment is applied).](https://www.tradingview.com/pine-script-reference/v6/#const_Constantforsplitsadjustmenttypesplitsadjustmentisapplied.)

Constant for splits adjustment type (splits adjustment is applied).
Type
const string
See also
adjustment.none
adjustment.dividends
ticker.new()
alert.freq_all

## [A named constant for use with the freq parameter of the alert() function.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforusewiththefreqparameterofthealertfunction.)

A named constant for use with the freq parameter of the alert() function.
All function calls trigger the alert.
Type
const string
See also
alert()
alert.freq_once_per_bar

## [A named constant for use with the freq parameter of the alert() function.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforusewiththefreqparameterofthealertfunction.)

A named constant for use with the freq parameter of the alert() function.
The first function call during the bar triggers the alert.
Type
const string
See also
alert()
alert.freq_once_per_bar_close

## [A named constant for use with the freq parameter of the alert() function.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforusewiththefreqparameterofthealertfunction.)

A named constant for use with the freq parameter of the alert() function.
The function call triggers the alert only when it occurs during the last script iteration of the real-time bar, when it closes.
Type
const string
See also
alert()
backadjustment.inherit

## [A constant to specify the value of the backadjustment parameter in ticker.new() and ticker.modify() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Aconstanttospecifythevalueofthebackadjustmentparameterinticker.newandticker.modifyfunctions.)

A constant to specify the value of the backadjustment parameter in ticker.new() and ticker.modify() functions.
Type
const backadjustment
See also
ticker.new()
ticker.modify()
backadjustment.on
backadjustment.off
backadjustment.off

## [A constant to specify the value of the backadjustment parameter in ticker.new() and ticker.modify() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Aconstanttospecifythevalueofthebackadjustmentparameterinticker.newandticker.modifyfunctions.)

A constant to specify the value of the backadjustment parameter in ticker.new() and ticker.modify() functions.
Type
const backadjustment
See also
ticker.new()
ticker.modify()
backadjustment.on
backadjustment.inherit
backadjustment.on

## [A constant to specify the value of the backadjustment parameter in ticker.new() and ticker.modify() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Aconstanttospecifythevalueofthebackadjustmentparameterinticker.newandticker.modifyfunctions.)

A constant to specify the value of the backadjustment parameter in ticker.new() and ticker.modify() functions.
Type
const backadjustment
See also
ticker.new()
ticker.modify()
backadjustment.inherit
backadjustment.off
barmerge.gaps_off

## [Merge strategy for requested data. Data is merged continuously without gaps, all the gaps are filled with the previous nearest existing value.](https://www.tradingview.com/pine-script-reference/v6/#const_Mergestrategyforrequesteddata.Dataismergedcontinuouslywithoutgapsallthegapsarefilledwiththepreviousnearestexistingvalue.)

Merge strategy for requested data. Data is merged continuously without gaps, all the gaps are filled with the previous nearest existing value.
Type
const barmerge_gaps
See also
request.security()
barmerge.gaps_on
barmerge.gaps_on

## [Merge strategy for requested data. Data is merged with possible gaps (na values).](https://www.tradingview.com/pine-script-reference/v6/#const_Mergestrategyforrequesteddata.Dataismergedwithpossiblegapsnavalues.)

Merge strategy for requested data. Data is merged with possible gaps (na values).
Type
const barmerge_gaps
See also
request.security()
barmerge.gaps_off
barmerge.lookahead_off

## [Merge strategy for the requested data position. Requested barset is merged with current barset in the order of sorting bars by their close time. This merge strategy disables effect of getting data from "future" on calculation on history.](https://www.tradingview.com/pine-script-reference/v6/#const_Mergestrategyfortherequesteddataposition.Requestedbarsetismergedwithcurrentbarsetintheorderofsortingbarsbytheirclosetime.Thismergestrategydisableseffectofgettingdatafromfutureoncalculationonhistory.)

Merge strategy for the requested data position. Requested barset is merged with current barset in the order of sorting bars by their close time. This merge strategy disables effect of getting data from "future" on calculation on history.
Type
const barmerge_lookahead
See also
request.security()
barmerge.lookahead_on
barmerge.lookahead_on

## [Merge strategy for the requested data position. Requested barset is merged with current barset in the order of sorting bars by their opening time. This merge strategy can lead to undesirable effect of getting data from "future" on calculation on history. This is unacceptable in backtesting strategies, but can be useful in indicators.](https://www.tradingview.com/pine-script-reference/v6/#const_Mergestrategyfortherequesteddataposition.Requestedbarsetismergedwithcurrentbarsetintheorderofsortingbarsbytheiropeningtime.Thismergestrategycanleadtoundesirableeffectofgettingdatafromfutureoncalculationonhistory.Thisisunacceptableinbacktestingstrategiesbutcanbeusefulinindicators.)

Merge strategy for the requested data position. Requested barset is merged with current barset in the order of sorting bars by their opening time. This merge strategy can lead to undesirable effect of getting data from "future" on calculation on history. This is unacceptable in backtesting strategies, but can be useful in indicators.
Type
const barmerge_lookahead
See also
request.security()
barmerge.lookahead_off

## [color.aqua](https://www.tradingview.com/pine-script-reference/v6/#const_color.aqua)

color.aqua

## [Is a named constant for #00BCD4 color.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantfor00BCD4color.)

Is a named constant for #00BCD4 color.
Type
const color
See also
color.black
color.silver
color.gray
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.navy
color.blue
color.teal
color.orange
color.black

## [Is a named constant for #363A45 color.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantfor363A45color.)

Is a named constant for #363A45 color.
Type
const color
See also
color.silver
color.gray
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.orange
color.blue

## [Is a named constant for #2962ff color.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantfor2962ffcolor.)

Is a named constant for #2962ff color.
Type
const color
See also
color.black
color.silver
color.gray
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.navy
color.teal
color.aqua
color.orange
color.fuchsia

## [Is a named constant for #E040FB color.](https://www.tradingview.com/pine-script-reference/v6/#const_IsanamedconstantforE040FBcolor.)

Is a named constant for #E040FB color.
Type
const color
See also
color.black
color.silver
color.gray
color.white
color.maroon
color.red
color.purple
color.green
color.lime
color.olive
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.orange
color.gray

## [Is a named constant for #787B86 color.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantfor787B86color.)

Is a named constant for #787B86 color.
Type
const color
See also
color.black
color.silver
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.orange
color.green

## [Is a named constant for #4CAF50 color.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantfor4CAF50color.)

Is a named constant for #4CAF50 color.
Type
const color
See also
color.black
color.silver
color.gray
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.lime
color.olive
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.orange
color.lime

## [Is a named constant for #00E676 color.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantfor00E676color.)

Is a named constant for #00E676 color.
Type
const color
See also
color.black
color.silver
color.gray
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.olive
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.orange
color.maroon

## [Is a named constant for #880E4F color.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantfor880E4Fcolor.)

Is a named constant for #880E4F color.
Type
const color
See also
color.black
color.silver
color.gray
color.white
color.red
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.orange
color.navy

## [Is a named constant for #311B92 color.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantfor311B92color.)

Is a named constant for #311B92 color.
Type
const color
See also
color.black
color.silver
color.gray
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.blue
color.teal
color.aqua
color.orange
color.olive

## [Is a named constant for #808000 color.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantfor808000color.)

Is a named constant for #808000 color.
Type
const color
See also
color.black
color.silver
color.gray
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.lime
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.orange
color.orange

## [Is a named constant for #FF9800 color.](https://www.tradingview.com/pine-script-reference/v6/#const_IsanamedconstantforFF9800color.)

Is a named constant for #FF9800 color.
Type
const color
See also
color.black
color.silver
color.gray
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.purple

## [Is a named constant for #9C27B0 color.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantfor9C27B0color.)

Is a named constant for #9C27B0 color.
Type
const color
See also
color.black
color.silver
color.gray
color.white
color.maroon
color.red
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.orange
color.red

## [Is a named constant for #F23645 color.](https://www.tradingview.com/pine-script-reference/v6/#const_IsanamedconstantforF23645color.)

Is a named constant for #F23645 color.
Type
const color
See also
color.black
color.silver
color.gray
color.white
color.maroon
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.orange
color.silver

## [Is a named constant for #B2B5BE color.](https://www.tradingview.com/pine-script-reference/v6/#const_IsanamedconstantforB2B5BEcolor.)

Is a named constant for #B2B5BE color.
Type
const color
See also
color.black
color.gray
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.orange
color.teal

## [Is a named constant for #089981 color.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantfor089981color.)

Is a named constant for #089981 color.
Type
const color
See also
color.black
color.silver
color.gray
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.navy
color.blue
color.aqua
color.orange
color.white

## [Is a named constant for #FFFFFF color.](https://www.tradingview.com/pine-script-reference/v6/#const_IsanamedconstantforFFFFFFcolor.)

Is a named constant for #FFFFFF color.
Type
const color
See also
color.black
color.silver
color.gray
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.yellow
color.navy
color.blue
color.teal
color.aqua
color.orange
color.yellow

## [Is a named constant for #FDD835 color.](https://www.tradingview.com/pine-script-reference/v6/#const_IsanamedconstantforFDD835color.)

Is a named constant for #FDD835 color.
Type
const color
See also
color.black
color.silver
color.gray
color.white
color.maroon
color.red
color.purple
color.fuchsia
color.green
color.lime
color.olive
color.navy
color.blue
color.teal
color.aqua
color.orange
currency.AED

## [Arab Emirates Dirham.](https://www.tradingview.com/pine-script-reference/v6/#const_ArabEmiratesDirham.)

Arab Emirates Dirham.
Type
const string
See also
strategy()
currency.ARS

## [Argentine Pesos.](https://www.tradingview.com/pine-script-reference/v6/#const_ArgentinePesos.)

Argentine Pesos.
Type
const string
See also
strategy()
currency.AUD

## [Australian dollar.](https://www.tradingview.com/pine-script-reference/v6/#const_Australiandollar.)

Australian dollar.
Type
const string
See also
strategy()
currency.BDT

## [Bangladeshi Taka.](https://www.tradingview.com/pine-script-reference/v6/#const_BangladeshiTaka.)

Bangladeshi Taka.
Type
const string
See also
strategy()
currency.BHD

## [Bahraini Dinar.](https://www.tradingview.com/pine-script-reference/v6/#const_BahrainiDinar.)

Bahraini Dinar.
Type
const string
See also
strategy()
currency.BRL

## [Brazilian real.](https://www.tradingview.com/pine-script-reference/v6/#const_Brazilianreal.)

Brazilian real.
Type
const string
See also
strategy()
currency.BTC

## [Bitcoin.](https://www.tradingview.com/pine-script-reference/v6/#const_Bitcoin.)

Bitcoin.
Type
const string
See also
strategy()
currency.CAD

## [Canadian dollar.](https://www.tradingview.com/pine-script-reference/v6/#const_Canadiandollar.)

Canadian dollar.
Type
const string
See also
strategy()
currency.CHF

## [Swiss franc.](https://www.tradingview.com/pine-script-reference/v6/#const_Swissfranc.)

Swiss franc.
Type
const string
See also
strategy()
currency.CLP

## [Chilean Peso.](https://www.tradingview.com/pine-script-reference/v6/#const_ChileanPeso.)

Chilean Peso.
Type
const string
See also
strategy()
currency.CNY

## [Chinese Yuan.](https://www.tradingview.com/pine-script-reference/v6/#const_ChineseYuan.)

Chinese Yuan.
Type
const string
See also
strategy()
currency.COP

## [Colombian Peso.](https://www.tradingview.com/pine-script-reference/v6/#const_ColombianPeso.)

Colombian Peso.
Type
const string
See also
strategy()
currency.CZK

## [Czech Koruna.](https://www.tradingview.com/pine-script-reference/v6/#const_CzechKoruna.)

Czech Koruna.
Type
const string
See also
strategy()
currency.DKK

## [Danish Krone.](https://www.tradingview.com/pine-script-reference/v6/#const_DanishKrone.)

Danish Krone.
Type
const string
See also
strategy()
currency.EGP

## [Egyptian pound.](https://www.tradingview.com/pine-script-reference/v6/#const_Egyptianpound.)

Egyptian pound.
Type
const string
See also
strategy()
currency.ETH

## [Ethereum.](https://www.tradingview.com/pine-script-reference/v6/#const_Ethereum.)

Ethereum.
Type
const string
See also
strategy()
currency.EUR

## [Euro.](https://www.tradingview.com/pine-script-reference/v6/#const_Euro.)

Euro.
Type
const string
See also
strategy()
currency.GBP

## [Pound sterling.](https://www.tradingview.com/pine-script-reference/v6/#const_Poundsterling.)

Pound sterling.
Type
const string
See also
strategy()
currency.HKD

## [Hong Kong dollar.](https://www.tradingview.com/pine-script-reference/v6/#const_HongKongdollar.)

Hong Kong dollar.
Type
const string
See also
strategy()
currency.HUF

## [Hungarian Forint.](https://www.tradingview.com/pine-script-reference/v6/#const_HungarianForint.)

Hungarian Forint.
Type
const string
See also
strategy()
currency.IDR

## [Indonesian Rupiah.](https://www.tradingview.com/pine-script-reference/v6/#const_IndonesianRupiah.)

Indonesian Rupiah.
Type
const string
See also
strategy()
currency.ILS

## [Israeli New Shekel.](https://www.tradingview.com/pine-script-reference/v6/#const_IsraeliNewShekel.)

Israeli New Shekel.
Type
const string
See also
strategy()
currency.INR

## [Indian rupee.](https://www.tradingview.com/pine-script-reference/v6/#const_Indianrupee.)

Indian rupee.
Type
const string
See also
strategy()
currency.ISK

## [Icelandic Krona.](https://www.tradingview.com/pine-script-reference/v6/#const_IcelandicKrona.)

Icelandic Krona.
Type
const string
See also
strategy()
currency.JPY

## [Japanese yen.](https://www.tradingview.com/pine-script-reference/v6/#const_Japaneseyen.)

Japanese yen.
Type
const string
See also
strategy()
currency.KES

## [Kenyan Shilling.](https://www.tradingview.com/pine-script-reference/v6/#const_KenyanShilling.)

Kenyan Shilling.
Type
const string
See also
strategy()
currency.KRW

## [South Korean won.](https://www.tradingview.com/pine-script-reference/v6/#const_SouthKoreanwon.)

South Korean won.
Type
const string
See also
strategy()
currency.KWD

## [Kuwaiti Dinar.](https://www.tradingview.com/pine-script-reference/v6/#const_KuwaitiDinar.)

Kuwaiti Dinar.
Type
const string
See also
strategy()
currency.LKR

## [Sri Lankan Rupee.](https://www.tradingview.com/pine-script-reference/v6/#const_SriLankanRupee.)

Sri Lankan Rupee.
Type
const string
See also
strategy()
currency.MAD

## [Moroccan Dirham.](https://www.tradingview.com/pine-script-reference/v6/#const_MoroccanDirham.)

Moroccan Dirham.
Type
const string
See also
strategy()
currency.MXN

## [Mexican Peso.](https://www.tradingview.com/pine-script-reference/v6/#const_MexicanPeso.)

Mexican Peso.
Type
const string
See also
strategy()
currency.MYR

## [Malaysian ringgit.](https://www.tradingview.com/pine-script-reference/v6/#const_Malaysianringgit.)

Malaysian ringgit.
Type
const string
See also
strategy()
currency.NGN

## [Nigerian Naira.](https://www.tradingview.com/pine-script-reference/v6/#const_NigerianNaira.)

Nigerian Naira.
Type
const string
See also
strategy()
currency.NOK

## [Norwegian krone.](https://www.tradingview.com/pine-script-reference/v6/#const_Norwegiankrone.)

Norwegian krone.
Type
const string
See also
strategy()
currency.NONE

## [Unspecified currency.](https://www.tradingview.com/pine-script-reference/v6/#const_Unspecifiedcurrency.)

Unspecified currency.
Type
const string
See also
strategy()
currency.NZD

## [New Zealand dollar.](https://www.tradingview.com/pine-script-reference/v6/#const_NewZealanddollar.)

New Zealand dollar.
Type
const string
See also
strategy()
currency.PEN

## [Peruvian sol.](https://www.tradingview.com/pine-script-reference/v6/#const_Peruviansol.)

Peruvian sol.
Type
const string
See also
strategy()
currency.PHP

## [Philippine Peso.](https://www.tradingview.com/pine-script-reference/v6/#const_PhilippinePeso.)

Philippine Peso.
Type
const string
See also
strategy()
currency.PKR

## [Pakistani rupee.](https://www.tradingview.com/pine-script-reference/v6/#const_Pakistanirupee.)

Pakistani rupee.
Type
const string
See also
strategy()
currency.PLN

## [Polish zloty.](https://www.tradingview.com/pine-script-reference/v6/#const_Polishzloty.)

Polish zloty.
Type
const string
See also
strategy()
currency.QAR

## [Qatari Riyal.](https://www.tradingview.com/pine-script-reference/v6/#const_QatariRiyal.)

Qatari Riyal.
Type
const string
See also
strategy()
currency.RON

## [Romanian Leu.](https://www.tradingview.com/pine-script-reference/v6/#const_RomanianLeu.)

Romanian Leu.
Type
const string
See also
strategy()
currency.RSD

## [Serbian Dinar.](https://www.tradingview.com/pine-script-reference/v6/#const_SerbianDinar.)

Serbian Dinar.
Type
const string
See also
strategy()
currency.RUB

## [Russian ruble.](https://www.tradingview.com/pine-script-reference/v6/#const_Russianruble.)

Russian ruble.
Type
const string
See also
strategy()
currency.SAR

## [Saudi Riyal.](https://www.tradingview.com/pine-script-reference/v6/#const_SaudiRiyal.)

Saudi Riyal.
Type
const string
See also
strategy()
currency.SEK

## [Swedish krona.](https://www.tradingview.com/pine-script-reference/v6/#const_Swedishkrona.)

Swedish krona.
Type
const string
See also
strategy()
currency.SGD

## [Singapore dollar.](https://www.tradingview.com/pine-script-reference/v6/#const_Singaporedollar.)

Singapore dollar.
Type
const string
See also
strategy()
currency.THB

## [Thai Baht.](https://www.tradingview.com/pine-script-reference/v6/#const_ThaiBaht.)

Thai Baht.
Type
const string
See also
strategy()
currency.TND

## [Tunisian Dinar.](https://www.tradingview.com/pine-script-reference/v6/#const_TunisianDinar.)

Tunisian Dinar.
Type
const string
See also
strategy()
currency.TRY

## [Turkish lira.](https://www.tradingview.com/pine-script-reference/v6/#const_Turkishlira.)

Turkish lira.
Type
const string
See also
strategy()
currency.TWD

## [New Taiwan Dollar.](https://www.tradingview.com/pine-script-reference/v6/#const_NewTaiwanDollar.)

New Taiwan Dollar.
Type
const string
See also
strategy()
currency.USD

## [United States dollar.](https://www.tradingview.com/pine-script-reference/v6/#const_UnitedStatesdollar.)

United States dollar.
Type
const string
See also
strategy()
currency.USDT

## [Tether.](https://www.tradingview.com/pine-script-reference/v6/#const_Tether.)

Tether.
Type
const string
See also
strategy()
currency.VES

## [Venezuelan Bolivar.](https://www.tradingview.com/pine-script-reference/v6/#const_VenezuelanBolivar.)

Venezuelan Bolivar.
Type
const string
See also
strategy()
currency.VND

## [Vietnamese Dong.](https://www.tradingview.com/pine-script-reference/v6/#const_VietnameseDong.)

Vietnamese Dong.
Type
const string
See also
strategy()
currency.ZAR

## [South African rand.](https://www.tradingview.com/pine-script-reference/v6/#const_SouthAfricanrand.)

South African rand.
Type
const string
See also
strategy()
dayofweek.friday

## [Is a named constant for return value of dayofweek() function and value of dayofweek variable.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantforreturnvalueofdayofweekfunctionandvalueofdayofweekvariable.)

Is a named constant for return value of dayofweek() function and value of dayofweek variable.
Type
const int
See also
dayofweek.sunday
dayofweek.monday
dayofweek.tuesday
dayofweek.wednesday
dayofweek.thursday
dayofweek.saturday
dayofweek.monday

## [Is a named constant for return value of dayofweek() function and value of dayofweek variable.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantforreturnvalueofdayofweekfunctionandvalueofdayofweekvariable.)

Is a named constant for return value of dayofweek() function and value of dayofweek variable.
Type
const int
See also
dayofweek.sunday
dayofweek.tuesday
dayofweek.wednesday
dayofweek.thursday
dayofweek.friday
dayofweek.saturday
dayofweek.saturday

## [Is a named constant for return value of dayofweek() function and value of dayofweek variable.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantforreturnvalueofdayofweekfunctionandvalueofdayofweekvariable.)

Is a named constant for return value of dayofweek() function and value of dayofweek variable.
Type
const int
See also
dayofweek.sunday
dayofweek.monday
dayofweek.tuesday
dayofweek.wednesday
dayofweek.thursday
dayofweek.friday
dayofweek.sunday

## [Is a named constant for return value of dayofweek() function and value of dayofweek variable.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantforreturnvalueofdayofweekfunctionandvalueofdayofweekvariable.)

Is a named constant for return value of dayofweek() function and value of dayofweek variable.
Type
const int
See also
dayofweek.monday
dayofweek.tuesday
dayofweek.wednesday
dayofweek.thursday
dayofweek.friday
dayofweek.saturday
dayofweek.thursday

## [Is a named constant for return value of dayofweek() function and value of dayofweek variable.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantforreturnvalueofdayofweekfunctionandvalueofdayofweekvariable.)

Is a named constant for return value of dayofweek() function and value of dayofweek variable.
Type
const int
See also
dayofweek.sunday
dayofweek.monday
dayofweek.tuesday
dayofweek.wednesday
dayofweek.friday
dayofweek.saturday
dayofweek.tuesday

## [Is a named constant for return value of dayofweek() function and value of dayofweek variable.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantforreturnvalueofdayofweekfunctionandvalueofdayofweekvariable.)

Is a named constant for return value of dayofweek() function and value of dayofweek variable.
Type
const int
See also
dayofweek.sunday
dayofweek.monday
dayofweek.wednesday
dayofweek.thursday
dayofweek.friday
dayofweek.saturday
dayofweek.wednesday

## [Is a named constant for return value of dayofweek() function and value of dayofweek variable.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantforreturnvalueofdayofweekfunctionandvalueofdayofweekvariable.)

Is a named constant for return value of dayofweek() function and value of dayofweek variable.
Type
const int
See also
dayofweek.sunday
dayofweek.monday
dayofweek.tuesday
dayofweek.thursday
dayofweek.friday
dayofweek.saturday
display.all

## [A named constant for use with the display parameter of the plot*(), input*(), fill(), bgcolor(), barcolor(), and hline() functions. Specifies that the values or visuals appear in all possible locations by default.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforusewiththedisplayparameteroftheplotinputfillbgcolorbarcolorandhlinefunctions.Specifiesthatthevaluesorvisualsappearinallpossiblelocationsbydefault.)

A named constant for use with the display parameter of the plot*(), input*(), fill(), bgcolor(), barcolor(), and hline() functions. Specifies that the values or visuals appear in all possible locations by default.
Type
const plot_simple_display
Remarks
The display.* constants support + and - operations, enabling custom combinations of display settings. For example, display.all - display.data_window specifies that the data for an input or plot appears in all possible locations except for the Data Window.
Selecting a deselected plot in the script's "Settings/Style" tab changes its display settings, causing the plotted data to appear in all available chart locations. To restore the display settings coded in the script, select "Reset settings" from the "Defaults" dropdown menu at the bottom of the "Settings" dialog box.
See also
plot()
plotshape()
plotchar()
plotarrow()
plotbar()
plotcandle()
display.data_window

## [A named constant for use with the display parameter of the plot*() and input*() functions. Specifies that the values are available in the Data Window by default. The Data Window tab is accessible by clicking the "Object Tree and Data Window" icon in the chart's right sidebar.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforusewiththedisplayparameteroftheplotandinputfunctions.SpecifiesthatthevaluesareavailableintheDataWindowbydefault.TheDataWindowtabisaccessiblebyclickingtheObjectTreeandDataWindowiconinthechartsrightsidebar.)

A named constant for use with the display parameter of the plot*() and input*() functions. Specifies that the values are available in the Data Window by default. The Data Window tab is accessible by clicking the "Object Tree and Data Window" icon in the chart's right sidebar.
Type
const plot_display
Remarks
The display.* constants support + and - operations, enabling custom combinations of display settings. For example, display.data_window + display.status_line specifies that the data for an input or plot appears in the Data Window and the script's status line, and display.all - display.data_window specifies that the data appears in all possible locations except for the Data Window.
Selecting a deselected plot in the script's "Settings/Style" tab changes its display settings, causing the plotted data to appear in all available chart locations. To restore the display settings coded in the script, select "Reset settings" from the "Defaults" dropdown menu at the bottom of the "Settings" dialog box.
See also
plot()
plotshape()
plotchar()
plotarrow()
plotbar()
plotcandle()
display.none

## [A named constant for use with the display parameter of the plot*(), input*(), fill(), bgcolor(), barcolor(), and hline() functions. Specifies that the values or visuals are not displayed anywhere by default.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforusewiththedisplayparameteroftheplotinputfillbgcolorbarcolorandhlinefunctions.Specifiesthatthevaluesorvisualsarenotdisplayedanywherebydefault.)

A named constant for use with the display parameter of the plot*(), input*(), fill(), bgcolor(), barcolor(), and hline() functions. Specifies that the values or visuals are not displayed anywhere by default.
Type
const plot_simple_display
Remarks
Selecting a deselected plot in the script's "Settings/Style" tab changes its display settings, causing the plotted data to appear in all available chart locations. To restore the display settings coded in the script, select "Reset settings" from the "Defaults" dropdown menu at the bottom of the "Settings" dialog box.
See also
plot()
plotshape()
plotchar()
plotarrow()
plotbar()
plotcandle()
display.pane

## [A named constant for use with the display parameter of the plot*() functions. Specifies that the plotted values are displayed in a chart pane by default.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforusewiththedisplayparameteroftheplotfunctions.Specifiesthattheplottedvaluesaredisplayedinachartpanebydefault.)

A named constant for use with the display parameter of the plot*() functions. Specifies that the plotted values are displayed in a chart pane by default.
Type
const plot_display
Remarks
The display.* constants support + and - operations, enabling custom combinations of display settings. For example, display.pane + display.data_window specifies that the plot's values appear in the chart pane and the Data Window, and display.all - display.pane specifies that the values appear in all possible locations except for the chart pane.
Selecting a deselected plot in the script's "Settings/Style" tab changes its display settings, causing the plotted data to appear in all available chart locations. To restore the display settings coded in the script, select "Reset settings" from the "Defaults" dropdown menu at the bottom of the "Settings" dialog box.
See also
plot()
plotshape()
plotchar()
plotarrow()
plotbar()
plotcandle()
display.pine_screener

## [A named constant for use with the display parameter of the plot() function. Specifies that, by default, the Pine Screener displays a column for the plot's values when the user applies the indicator to the chosen watchlist.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforusewiththedisplayparameteroftheplotfunction.SpecifiesthatbydefaultthePineScreenerdisplaysacolumnfortheplotsvalueswhentheuserappliestheindicatortothechosenwatchlist.)

A named constant for use with the display parameter of the plot() function. Specifies that, by default, the Pine Screener displays a column for the plot's values when the user applies the indicator to the chosen watchlist.
Type
const plot_display
Remarks
The display.* constants support + and - operations, enabling custom combinations of display settings. For example, display.data_window + display.pine_screener specifies that the plotted values appear in the Data Window and the Pine Screener, and display.all - display.pine_screener specifies that the values appear in all possible locations except for the Pine Screener.
The Pine Screener displays columns for only the first 10 enabled plots from a script by default. If a plot's default display settings do not include the screener, or if the screener already shows columns for 10 other plots from the script, users can configure the screener to show a column for the plot by using the "Manage columns" menu at the far right of the table header.
See also
plot()
plotshape()
plotchar()
plotarrow()
plotbar()
plotcandle()
display.price_scale

## [A named constant for use with the display parameter of the plot*() functions. Specifies that the price scale displays a label for the plot's data, but only if the chart's settings allow it.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforusewiththedisplayparameteroftheplotfunctions.Specifiesthatthepricescaledisplaysalabelfortheplotsdatabutonlyifthechartssettingsallowit.)

A named constant for use with the display parameter of the plot*() functions. Specifies that the price scale displays a label for the plot's data, but only if the chart's settings allow it.
Type
const plot_display
Remarks
The display.* constants support + and - operations, enabling custom combinations of display settings. For example, display.price_scale + display.data_window specifies that the plot's data appears on the price scale and in the Data Window, and display.all - display.price_scale specifies that the data appears in all possible locations except for the price scale.
Selecting a deselected plot in the script's "Settings/Style" tab changes its display settings, causing the plotted data to appear in all available chart locations. To restore the display settings coded in the script, select "Reset settings" from the "Defaults" dropdown menu at the bottom of the "Settings" dialog box.
See also
plot()
plotshape()
plotchar()
plotarrow()
plotbar()
plotcandle()
display.status_line

## [A named constant for use with the display parameter of the plot*() and input*() functions. Specifies that the values are available in the script's status line, but only if the chart's settings allow it.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforusewiththedisplayparameteroftheplotandinputfunctions.Specifiesthatthevaluesareavailableinthescriptsstatuslinebutonlyifthechartssettingsallowit.)

A named constant for use with the display parameter of the plot*() and input*() functions. Specifies that the values are available in the script's status line, but only if the chart's settings allow it.
Type
const plot_display
Remarks
The display.* constants support + and - operations, enabling custom combinations of display settings. For example, display.data_window + display.status_line specifies that the data for an input or plot appears in the Data Window and the script's status line, and display.all - display.status_line specifies that the data appears in all possible locations except for the status line.
Selecting a deselected plot in the script's "Settings/Style" tab changes its display settings, causing the plotted data to appear in all available chart locations. To restore the display settings coded in the script, select "Reset settings" from the "Defaults" dropdown menu at the bottom of the "Settings" dialog box.
See also
plot()
plotshape()
plotchar()
plotarrow()
plotbar()
plotcandle()
dividends.gross

## [A named constant for the request.dividends() function. Is used to request the dividends return on a stock before deductions.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantfortherequest.dividendsfunction.Isusedtorequestthedividendsreturnonastockbeforedeductions.)

A named constant for the request.dividends() function. Is used to request the dividends return on a stock before deductions.
Type
const string
See also
request.dividends()
dividends.net

## [A named constant for the request.dividends() function. Is used to request the dividends return on a stock after deductions.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantfortherequest.dividendsfunction.Isusedtorequestthedividendsreturnonastockafterdeductions.)

A named constant for the request.dividends() function. Is used to request the dividends return on a stock after deductions.
Type
const string
See also
request.dividends()
earnings.actual

## [A named constant for the request.earnings() function. Is used to request the earnings value as it was reported.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantfortherequest.earningsfunction.Isusedtorequesttheearningsvalueasitwasreported.)

A named constant for the request.earnings() function. Is used to request the earnings value as it was reported.
Type
const string
See also
request.earnings()
earnings.estimate

## [A named constant for the request.earnings() function. Is used to request the estimated earnings value.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantfortherequest.earningsfunction.Isusedtorequesttheestimatedearningsvalue.)

A named constant for the request.earnings() function. Is used to request the estimated earnings value.
Type
const string
See also
request.earnings()
earnings.standardized

## [A named constant for the request.earnings() function. Is used to request the standardized earnings value.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantfortherequest.earningsfunction.Isusedtorequestthestandardizedearningsvalue.)

A named constant for the request.earnings() function. Is used to request the standardized earnings value.
Type
const string
See also
request.earnings()
extend.both

## [A named constant for line.new() and line.set_extend() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforline.newandline.set_extendfunctions.)

A named constant for line.new() and line.set_extend() functions.
Type
const string
See also
line.new()
line.set_extend()
extend.none
extend.left
extend.right
extend.left

## [A named constant for line.new() and line.set_extend() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforline.newandline.set_extendfunctions.)

A named constant for line.new() and line.set_extend() functions.
Type
const string
See also
line.new()
line.set_extend()
extend.none
extend.right
extend.both
extend.none

## [A named constant for line.new() and line.set_extend() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforline.newandline.set_extendfunctions.)

A named constant for line.new() and line.set_extend() functions.
Type
const string
See also
line.new()
line.set_extend()
extend.left
extend.right
extend.both
extend.right

## [A named constant for line.new() and line.set_extend() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforline.newandline.set_extendfunctions.)

A named constant for line.new() and line.set_extend() functions.
Type
const string
See also
line.new()
line.set_extend()
extend.none
extend.left
extend.both
false

## [Literal representing a bool value, and result of a comparison operation.](https://www.tradingview.com/pine-script-reference/v6/#const_Literalrepresentingaboolvalueandresultofacomparisonoperation.)

Literal representing a bool value, and result of a comparison operation.
Remarks
See the User Manual for comparison operators and logical operators.
See also
bool
font.family_default

## [Default text font for box.new(), box.set_text_font_family(), label.new(), label.set_text_font_family(), table.cell() and table.cell_set_text_font_family() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Defaulttextfontforbox.newbox.set_text_font_familylabel.newlabel.set_text_font_familytable.cellandtable.cell_set_text_font_familyfunctions.)

Default text font for box.new(), box.set_text_font_family(), label.new(), label.set_text_font_family(), table.cell() and table.cell_set_text_font_family() functions.
Type
const string
See also
box.new()
box.set_text_font_family()
label.new()
label.set_text_font_family()
table.cell()
table.cell_set_text_font_family()
font.family_monospace
font.family_monospace

## [Monospace text font for box.new(), box.set_text_font_family(), label.new(), label.set_text_font_family(), table.cell() and table.cell_set_text_font_family() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Monospacetextfontforbox.newbox.set_text_font_familylabel.newlabel.set_text_font_familytable.cellandtable.cell_set_text_font_familyfunctions.)

Monospace text font for box.new(), box.set_text_font_family(), label.new(), label.set_text_font_family(), table.cell() and table.cell_set_text_font_family() functions.
Type
const string
See also
box.new()
box.set_text_font_family()
label.new()
label.set_text_font_family()
table.cell()
table.cell_set_text_font_family()
font.family_default
format.inherit

## [Is a named constant for selecting the formatting of the script output values from the parent series in the indicator() function.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantforselectingtheformattingofthescriptoutputvaluesfromtheparentseriesintheindicatorfunction.)

Is a named constant for selecting the formatting of the script output values from the parent series in the indicator() function.
Type
const string
See also
indicator()
format.price
format.volume
format.percent
format.mintick

## [Is a named constant to use with the str.tostring() function. Passing a number to str.tostring() with this argument rounds the number to the nearest value that can be divided by syminfo.mintick, without the remainder, with ties rounding up, and returns the string version of said value with trailing zeros.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstanttousewiththestr.tostringfunction.Passinganumbertostr.tostringwiththisargumentroundsthenumbertothenearestvaluethatcanbedividedbysyminfo.mintickwithouttheremainderwithtiesroundingupandreturnsthestringversionofsaidvaluewithtrailingzeros.)

Is a named constant to use with the str.tostring() function. Passing a number to str.tostring() with this argument rounds the number to the nearest value that can be divided by syminfo.mintick, without the remainder, with ties rounding up, and returns the string version of said value with trailing zeros.
Type
const string
See also
indicator()
format.inherit
format.price
format.volume
format.percent

## [Is a named constant for selecting the formatting of the script output values as a percentage in the indicator function. It adds a percent sign after values.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantforselectingtheformattingofthescriptoutputvaluesasapercentageintheindicatorfunction.Itaddsapercentsignaftervalues.)

Is a named constant for selecting the formatting of the script output values as a percentage in the indicator function. It adds a percent sign after values.
Type
const string
Remarks
The default precision is 2, regardless of the precision of the chart itself. This can be changed with the 'precision' argument of the indicator() function.
See also
indicator()
format.inherit
format.price
format.volume
format.price

## [Is a named constant for selecting the formatting of the script output values as prices in the indicator() function.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantforselectingtheformattingofthescriptoutputvaluesaspricesintheindicatorfunction.)

Is a named constant for selecting the formatting of the script output values as prices in the indicator() function.
Type
const string
Remarks
If format is format.price, default precision value is set. You can use the precision argument of indicator function to change the precision value.
See also
indicator()
format.inherit
format.volume
format.percent
format.volume

## [Is a named constant for selecting the formatting of the script output values as volume in the indicator() function, e.g. '5183' will be formatted as '5.183K'.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantforselectingtheformattingofthescriptoutputvaluesasvolumeintheindicatorfunctione.g.5183willbeformattedas5.183K.)

Is a named constant for selecting the formatting of the script output values as volume in the indicator() function, e.g. '5183' will be formatted as '5.183K'.
The decimal precision rules defined by this variable take precedence over other precision settings. When an indicator(), strategy(), or plot*() call uses this format option, the function's precision parameter will not affect the result.
Type
const string
See also
indicator()
format.inherit
format.price
format.percent
hline.style_dashed

## [Is a named constant for dashed linestyle of hline() function.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantfordashedlinestyleofhlinefunction.)

Is a named constant for dashed linestyle of hline() function.
Type
const hline_style
See also
hline.style_solid
hline.style_dotted
hline.style_dotted

## [Is a named constant for dotted linestyle of hline() function.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantfordottedlinestyleofhlinefunction.)

Is a named constant for dotted linestyle of hline() function.
Type
const hline_style
See also
hline.style_solid
hline.style_dashed
hline.style_solid

## [Is a named constant for solid linestyle of hline() function.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantforsolidlinestyleofhlinefunction.)

Is a named constant for solid linestyle of hline() function.
Type
const hline_style
See also
hline.style_dotted
hline.style_dashed
label.style_arrowdown

## [Label style for label.new() and label.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Labelstyleforlabel.newandlabel.set_stylefunctions.)

Label style for label.new() and label.set_style() functions.
Type
const string
See also
label.new()
label.set_style()
label.set_textalign()
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_arrowup

## [Label style for label.new() and label.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Labelstyleforlabel.newandlabel.set_stylefunctions.)

Label style for label.new() and label.set_style() functions.
Type
const string
See also
label.new()
label.set_style()
label.set_textalign()
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_circle

## [Label style for label.new() and label.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Labelstyleforlabel.newandlabel.set_stylefunctions.)

Label style for label.new() and label.set_style() functions.
Type
const string
See also
label.new()
label.set_style()
label.set_textalign()
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_cross

## [Label style for label.new() and label.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Labelstyleforlabel.newandlabel.set_stylefunctions.)

Label style for label.new() and label.set_style() functions.
Type
const string
See also
label.new()
label.set_style()
label.set_textalign()
label.style_none
label.style_xcross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_diamond

## [Label style for label.new() and label.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Labelstyleforlabel.newandlabel.set_stylefunctions.)

Label style for label.new() and label.set_style() functions.
Type
const string
See also
label.new()
label.set_style()
label.set_textalign()
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_flag

## [Label style for label.new() and label.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Labelstyleforlabel.newandlabel.set_stylefunctions.)

Label style for label.new() and label.set_style() functions.
Type
const string
See also
label.new()
label.set_style()
label.set_textalign()
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_label_center

## [Label style for label.new() and label.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Labelstyleforlabel.newandlabel.set_stylefunctions.)

Label style for label.new() and label.set_style() functions.
Type
const string
See also
label.new()
label.set_style()
label.set_textalign()
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_square
label.style_diamond
label.style_label_down

## [Label style for label.new() and label.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Labelstyleforlabel.newandlabel.set_stylefunctions.)

Label style for label.new() and label.set_style() functions.
Type
const string
See also
label.new()
label.set_style()
label.set_textalign()
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_label_left

## [Label style for label.new() and label.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Labelstyleforlabel.newandlabel.set_stylefunctions.)

Label style for label.new() and label.set_style() functions.
Type
const string
See also
label.new()
label.set_style()
label.set_textalign()
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_label_lower_left

## [Label style for label.new() and label.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Labelstyleforlabel.newandlabel.set_stylefunctions.)

Label style for label.new() and label.set_style() functions.
Type
const string
See also
label.new()
label.set_style()
label.set_textalign()
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_label_lower_right

## [Label style for label.new() and label.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Labelstyleforlabel.newandlabel.set_stylefunctions.)

Label style for label.new() and label.set_style() functions.
Type
const string
See also
label.new()
label.set_style()
label.set_textalign()
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_label_right

## [Label style for label.new() and label.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Labelstyleforlabel.newandlabel.set_stylefunctions.)

Label style for label.new() and label.set_style() functions.
Type
const string
See also
label.new()
label.set_style()
label.set_textalign()
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_label_up

## [Label style for label.new() and label.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Labelstyleforlabel.newandlabel.set_stylefunctions.)

Label style for label.new() and label.set_style() functions.
Type
const string
See also
label.new()
label.set_style()
label.set_textalign()
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_label_upper_left

## [Label style for label.new() and label.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Labelstyleforlabel.newandlabel.set_stylefunctions.)

Label style for label.new() and label.set_style() functions.
Type
const string
See also
label.new()
label.set_style()
label.set_textalign()
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_label_upper_right

## [Label style for label.new() and label.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Labelstyleforlabel.newandlabel.set_stylefunctions.)

Label style for label.new() and label.set_style() functions.
Type
const string
See also
label.new()
label.set_style()
label.set_textalign()
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_center
label.style_square
label.style_diamond
label.style_none

## [Label style for label.new() and label.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Labelstyleforlabel.newandlabel.set_stylefunctions.)

Label style for label.new() and label.set_style() functions.
Type
const string
See also
label.new()
label.set_style()
label.set_textalign()
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_center
label.style_square
label.style_diamond
label.style_square

## [Label style for label.new() and label.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Labelstyleforlabel.newandlabel.set_stylefunctions.)

Label style for label.new() and label.set_style() functions.
Type
const string
See also
label.new()
label.set_style()
label.set_textalign()
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_diamond
label.style_text_outline

## [Label style for label.new() and label.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Labelstyleforlabel.newandlabel.set_stylefunctions.)

Label style for label.new() and label.set_style() functions.
Type
const string
See also
label.new()
label.set_style()
label.set_textalign()
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_triangledown

## [Label style for label.new() and label.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Labelstyleforlabel.newandlabel.set_stylefunctions.)

Label style for label.new() and label.set_style() functions.
Type
const string
See also
label.new()
label.set_style()
label.set_textalign()
label.style_none
label.style_xcross
label.style_cross
label.style_triangleup
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_triangleup

## [Label style for label.new() and label.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Labelstyleforlabel.newandlabel.set_stylefunctions.)

Label style for label.new() and label.set_style() functions.
Type
const string
See also
label.new()
label.set_style()
label.set_textalign()
label.style_none
label.style_xcross
label.style_cross
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_lower_left
label.style_label_lower_right
label.style_label_upper_left
label.style_label_upper_right
label.style_label_center
label.style_square
label.style_diamond
label.style_xcross

## [Label style for label.new() and label.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Labelstyleforlabel.newandlabel.set_stylefunctions.)

Label style for label.new() and label.set_style() functions.
Type
const string
See also
label.new()
label.set_style()
label.set_textalign()
label.style_none
label.style_cross
label.style_triangleup
label.style_triangledown
label.style_flag
label.style_circle
label.style_arrowup
label.style_arrowdown
label.style_label_up
label.style_label_down
label.style_label_left
label.style_label_right
label.style_label_center
label.style_square
label.style_diamond
line.style_arrow_both

## [Line style for line.new() and line.set_style() functions. Solid line with arrows on both points.](https://www.tradingview.com/pine-script-reference/v6/#const_Linestyleforline.newandline.set_stylefunctions.Solidlinewitharrowsonbothpoints.)

Line style for line.new() and line.set_style() functions. Solid line with arrows on both points.
Type
const string
See also
line.new()
line.set_style()
line.style_solid
line.style_dotted
line.style_dashed
line.style_arrow_left
line.style_arrow_right
line.style_arrow_left

## [Line style for line.new() and line.set_style() functions. Solid line with arrow on the first point.](https://www.tradingview.com/pine-script-reference/v6/#const_Linestyleforline.newandline.set_stylefunctions.Solidlinewitharrowonthefirstpoint.)

Line style for line.new() and line.set_style() functions. Solid line with arrow on the first point.
Type
const string
See also
line.new()
line.set_style()
line.style_solid
line.style_dotted
line.style_dashed
line.style_arrow_right
line.style_arrow_both
line.style_arrow_right

## [Line style for line.new() and line.set_style() functions. Solid line with arrow on the second point.](https://www.tradingview.com/pine-script-reference/v6/#const_Linestyleforline.newandline.set_stylefunctions.Solidlinewitharrowonthesecondpoint.)

Line style for line.new() and line.set_style() functions. Solid line with arrow on the second point.
Type
const string
See also
line.new()
line.set_style()
line.style_solid
line.style_dotted
line.style_dashed
line.style_arrow_left
line.style_arrow_both
line.style_dashed

## [Line style for line.new() and line.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Linestyleforline.newandline.set_stylefunctions.)

Line style for line.new() and line.set_style() functions.
Type
const string
See also
line.new()
line.set_style()
line.style_solid
line.style_dotted
line.style_arrow_left
line.style_arrow_right
line.style_arrow_both
line.style_dotted

## [Line style for line.new() and line.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Linestyleforline.newandline.set_stylefunctions.)

Line style for line.new() and line.set_style() functions.
Type
const string
See also
line.new()
line.set_style()
line.style_solid
line.style_dashed
line.style_arrow_left
line.style_arrow_right
line.style_arrow_both
line.style_solid

## [Line style for line.new() and line.set_style() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Linestyleforline.newandline.set_stylefunctions.)

Line style for line.new() and line.set_style() functions.
Type
const string
See also
line.new()
line.set_style()
line.style_dotted
line.style_dashed
line.style_arrow_left
line.style_arrow_right
line.style_arrow_both
location.abovebar

## [Location value for plotshape(), plotchar() functions. Shape is plotted above main series bars.](https://www.tradingview.com/pine-script-reference/v6/#const_Locationvalueforplotshapeplotcharfunctions.Shapeisplottedabovemainseriesbars.)

Location value for plotshape(), plotchar() functions. Shape is plotted above main series bars.
Type
const string
See also
plotshape()
plotchar()
location.belowbar
location.top
location.bottom
location.absolute
location.absolute

## [Location value for plotshape(), plotchar() functions. Shape is plotted on chart using indicator value as a price coordinate.](https://www.tradingview.com/pine-script-reference/v6/#const_Locationvalueforplotshapeplotcharfunctions.Shapeisplottedonchartusingindicatorvalueasapricecoordinate.)

Location value for plotshape(), plotchar() functions. Shape is plotted on chart using indicator value as a price coordinate.
Type
const string
See also
plotshape()
plotchar()
location.abovebar
location.belowbar
location.top
location.bottom
location.belowbar

## [Location value for plotshape(), plotchar() functions. Shape is plotted below main series bars.](https://www.tradingview.com/pine-script-reference/v6/#const_Locationvalueforplotshapeplotcharfunctions.Shapeisplottedbelowmainseriesbars.)

Location value for plotshape(), plotchar() functions. Shape is plotted below main series bars.
Type
const string
See also
plotshape()
plotchar()
location.abovebar
location.top
location.bottom
location.absolute
location.bottom

## [Location value for plotshape(), plotchar() functions. Shape is plotted near the bottom chart border.](https://www.tradingview.com/pine-script-reference/v6/#const_Locationvalueforplotshapeplotcharfunctions.Shapeisplottednearthebottomchartborder.)

Location value for plotshape(), plotchar() functions. Shape is plotted near the bottom chart border.
Type
const string
See also
plotshape()
plotchar()
location.abovebar
location.belowbar
location.top
location.absolute
location.top

## [Location value for plotshape(), plotchar() functions. Shape is plotted near the top chart border.](https://www.tradingview.com/pine-script-reference/v6/#const_Locationvalueforplotshapeplotcharfunctions.Shapeisplottednearthetopchartborder.)

Location value for plotshape(), plotchar() functions. Shape is plotted near the top chart border.
Type
const string
See also
plotshape()
plotchar()
location.abovebar
location.belowbar
location.bottom
location.absolute
math.e

## [Is a named constant for Euler's number. It is equal to 2.7182818284590452.](https://www.tradingview.com/pine-script-reference/v6/#const_IsanamedconstantforEulersnumber.Itisequalto2.7182818284590452.)

Is a named constant for Euler's number. It is equal to 2.7182818284590452.
Type
const float
See also
math.phi
math.pi
math.rphi
math.phi

## [Is a named constant for the golden ratio. It is equal to 1.6180339887498948.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantforthegoldenratio.Itisequalto1.6180339887498948.)

Is a named constant for the golden ratio. It is equal to 1.6180339887498948.
Type
const float
See also
math.e
math.pi
math.rphi
math.pi

## [Is a named constant for Archimedes' constant. It is equal to 3.1415926535897932.](https://www.tradingview.com/pine-script-reference/v6/#const_IsanamedconstantforArchimedesconstant.Itisequalto3.1415926535897932.)

Is a named constant for Archimedes' constant. It is equal to 3.1415926535897932.
Type
const float
See also
math.e
math.phi
math.rphi
math.rphi

## [Is a named constant for the golden ratio conjugate. It is equal to 0.6180339887498948.](https://www.tradingview.com/pine-script-reference/v6/#const_Isanamedconstantforthegoldenratioconjugate.Itisequalto0.6180339887498948.)

Is a named constant for the golden ratio conjugate. It is equal to 0.6180339887498948.
Type
const float
See also
math.e
math.pi
math.phi
order.ascending

## [Determines the sort order of the array from the smallest to the largest value.](https://www.tradingview.com/pine-script-reference/v6/#const_Determinesthesortorderofthearrayfromthesmallesttothelargestvalue.)

Determines the sort order of the array from the smallest to the largest value.
Type
const sort_order
See also
array.new_float()
array.sort()
order.descending

## [Determines the sort order of the array from the largest to the smallest value.](https://www.tradingview.com/pine-script-reference/v6/#const_Determinesthesortorderofthearrayfromthelargesttothesmallestvalue.)

Determines the sort order of the array from the largest to the smallest value.
Type
const sort_order
See also
array.new_float()
array.sort()
plot.linestyle_dashed

## [A named constant for use with the plot() function's linestyle parameter, which modifies the appearance of plotted lines. If the style argument of the function call specifies a plot style that displays a line, using this constant as the linestyle argument specifies that the plotted line is dashed.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforusewiththeplotfunctionslinestyleparameterwhichmodifiestheappearanceofplottedlines.Ifthestyleargumentofthefunctioncallspecifiesaplotstylethatdisplaysalineusingthisconstantasthelinestyleargumentspecifiesthattheplottedlineisdashed.)

A named constant for use with the plot() function's linestyle parameter, which modifies the appearance of plotted lines. If the style argument of the function call specifies a plot style that displays a line, using this constant as the linestyle argument specifies that the plotted line is dashed.
Type
const plot_line_style
See also
plot()
plot.linestyle_solid
plot.linestyle_dotted
plot.linestyle_dotted

## [A named constant for use with the plot() function's linestyle parameter, which modifies the appearance of plotted lines. If the style argument of the function call specifies a plot style that displays a line, using this constant as the linestyle argument specifies that the plotted line is dotted.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforusewiththeplotfunctionslinestyleparameterwhichmodifiestheappearanceofplottedlines.Ifthestyleargumentofthefunctioncallspecifiesaplotstylethatdisplaysalineusingthisconstantasthelinestyleargumentspecifiesthattheplottedlineisdotted.)

A named constant for use with the plot() function's linestyle parameter, which modifies the appearance of plotted lines. If the style argument of the function call specifies a plot style that displays a line, using this constant as the linestyle argument specifies that the plotted line is dotted.
Type
const plot_line_style
See also
plot()
plot.linestyle_dashed
plot.linestyle_solid
plot.linestyle_solid

## [A named constant for use with the plot() function's linestyle parameter, which modifies the appearance of plotted lines. If the style argument of the function call specifies a plot style that displays a line, using this constant as the linestyle argument specifies that the plotted line is solid.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforusewiththeplotfunctionslinestyleparameterwhichmodifiestheappearanceofplottedlines.Ifthestyleargumentofthefunctioncallspecifiesaplotstylethatdisplaysalineusingthisconstantasthelinestyleargumentspecifiesthattheplottedlineissolid.)

A named constant for use with the plot() function's linestyle parameter, which modifies the appearance of plotted lines. If the style argument of the function call specifies a plot style that displays a line, using this constant as the linestyle argument specifies that the plotted line is solid.
Type
const plot_line_style
See also
plot()
plot.linestyle_dashed
plot.linestyle_dotted
plot.style_area

## [A named constant for the 'Area' style, to be used as an argument for the style parameter in the plot() function.](https://www.tradingview.com/pine-script-reference/v6/#const_AnamedconstantfortheAreastyletobeusedasanargumentforthestyleparameterintheplotfunction.)

A named constant for the 'Area' style, to be used as an argument for the style parameter in the plot() function.
Type
const plot_style
See also
plot()
plot.style_line
plot.style_linebr
plot.style_histogram
plot.style_cross
plot.style_areabr
plot.style_columns
plot.style_circles
plot.style_stepline
plot.style_stepline_diamond
plot.style_steplinebr
plot.style_areabr

## [A named constant for the 'Area With Breaks' style, to be used as an argument for the style parameter in the plot() function. Similar to plot.style_area, except the gaps in the data are not filled.](https://www.tradingview.com/pine-script-reference/v6/#const_AnamedconstantfortheAreaWithBreaksstyletobeusedasanargumentforthestyleparameterintheplotfunction.Similartoplot.style_areaexceptthegapsinthedataarenotfilled.)

A named constant for the 'Area With Breaks' style, to be used as an argument for the style parameter in the plot() function. Similar to plot.style_area, except the gaps in the data are not filled.
Type
const plot_style
See also
plot()
plot.style_line
plot.style_linebr
plot.style_histogram
plot.style_cross
plot.style_area
plot.style_columns
plot.style_circles
plot.style_stepline
plot.style_stepline_diamond
plot.style_steplinebr
plot.style_circles

## [A named constant for the 'Circles' style, to be used as an argument for the style parameter in the plot() function.](https://www.tradingview.com/pine-script-reference/v6/#const_AnamedconstantfortheCirclesstyletobeusedasanargumentforthestyleparameterintheplotfunction.)

A named constant for the 'Circles' style, to be used as an argument for the style parameter in the plot() function.
Type
const plot_style
See also
plot()
plot.style_line
plot.style_linebr
plot.style_histogram
plot.style_cross
plot.style_area
plot.style_areabr
plot.style_columns
plot.style_stepline
plot.style_stepline_diamond
plot.style_steplinebr
plot.style_columns

## [A named constant for the 'Columns' style, to be used as an argument for the style parameter in the plot() function.](https://www.tradingview.com/pine-script-reference/v6/#const_AnamedconstantfortheColumnsstyletobeusedasanargumentforthestyleparameterintheplotfunction.)

A named constant for the 'Columns' style, to be used as an argument for the style parameter in the plot() function.
Type
const plot_style
See also
plot()
plot.style_line
plot.style_linebr
plot.style_histogram
plot.style_cross
plot.style_area
plot.style_areabr
plot.style_circles
plot.style_stepline
plot.style_stepline_diamond
plot.style_steplinebr
plot.style_cross

## [A named constant for the 'Cross' style, to be used as an argument for the style parameter in the plot() function.](https://www.tradingview.com/pine-script-reference/v6/#const_AnamedconstantfortheCrossstyletobeusedasanargumentforthestyleparameterintheplotfunction.)

A named constant for the 'Cross' style, to be used as an argument for the style parameter in the plot() function.
Type
const plot_style
See also
plot()
plot.style_line
plot.style_linebr
plot.style_histogram
plot.style_area
plot.style_areabr
plot.style_columns
plot.style_circles
plot.style_stepline
plot.style_stepline_diamond
plot.style_steplinebr
plot.style_histogram

## [A named constant for the 'Histogram' style, to be used as an argument for the style parameter in the plot() function.](https://www.tradingview.com/pine-script-reference/v6/#const_AnamedconstantfortheHistogramstyletobeusedasanargumentforthestyleparameterintheplotfunction.)

A named constant for the 'Histogram' style, to be used as an argument for the style parameter in the plot() function.
Type
const plot_style
See also
plot()
plot.style_line
plot.style_linebr
plot.style_cross
plot.style_area
plot.style_areabr
plot.style_columns
plot.style_circles
plot.style_stepline
plot.style_stepline_diamond
plot.style_steplinebr
plot.style_line

## [A named constant for the 'Line' style, to be used as an argument for the style parameter in the plot() function.](https://www.tradingview.com/pine-script-reference/v6/#const_AnamedconstantfortheLinestyletobeusedasanargumentforthestyleparameterintheplotfunction.)

A named constant for the 'Line' style, to be used as an argument for the style parameter in the plot() function.
Type
const plot_style
See also
plot()
plot.style_linebr
plot.style_histogram
plot.style_cross
plot.style_area
plot.style_areabr
plot.style_columns
plot.style_circles
plot.style_stepline
plot.style_stepline_diamond
plot.style_steplinebr
plot.style_linebr

## [A named constant for the 'Line With Breaks' style, to be used as an argument for the style parameter in the plot() function. Similar to plot.style_line, except the gaps in the data are not filled.](https://www.tradingview.com/pine-script-reference/v6/#const_AnamedconstantfortheLineWithBreaksstyletobeusedasanargumentforthestyleparameterintheplotfunction.Similartoplot.style_lineexceptthegapsinthedataarenotfilled.)

A named constant for the 'Line With Breaks' style, to be used as an argument for the style parameter in the plot() function. Similar to plot.style_line, except the gaps in the data are not filled.
Type
const plot_style
See also
plot()
plot.style_line
plot.style_histogram
plot.style_cross
plot.style_area
plot.style_areabr
plot.style_columns
plot.style_circles
plot.style_stepline
plot.style_stepline_diamond
plot.style_steplinebr
plot.style_stepline

## [A named constant for the 'Step Line' style, to be used as an argument for the style parameter in the plot() function.](https://www.tradingview.com/pine-script-reference/v6/#const_AnamedconstantfortheStepLinestyletobeusedasanargumentforthestyleparameterintheplotfunction.)

A named constant for the 'Step Line' style, to be used as an argument for the style parameter in the plot() function.
Type
const plot_style
See also
plot()
plot.style_line
plot.style_linebr
plot.style_histogram
plot.style_cross
plot.style_area
plot.style_areabr
plot.style_columns
plot.style_circles
plot.style_stepline_diamond
plot.style_steplinebr
plot.style_stepline_diamond

## [A named constant for the 'Step Line With Diamonds' style, to be used as an argument for the style parameter in the plot() function. Similar to plot.style_stepline, except the data changes are also marked with the Diamond shapes.](https://www.tradingview.com/pine-script-reference/v6/#const_AnamedconstantfortheStepLineWithDiamondsstyletobeusedasanargumentforthestyleparameterintheplotfunction.Similartoplot.style_steplineexceptthedatachangesarealsomarkedwiththeDiamondshapes.)

A named constant for the 'Step Line With Diamonds' style, to be used as an argument for the style parameter in the plot() function. Similar to plot.style_stepline, except the data changes are also marked with the Diamond shapes.
Type
const plot_style
See also
plot()
plot.style_line
plot.style_linebr
plot.style_histogram
plot.style_cross
plot.style_area
plot.style_areabr
plot.style_columns
plot.style_circles
plot.style_stepline
plot.style_steplinebr
plot.style_steplinebr

## [A named constant for the 'Step line with Breaks' style, to be used as an argument for the style parameter in the plot() function.](https://www.tradingview.com/pine-script-reference/v6/#const_AnamedconstantfortheSteplinewithBreaksstyletobeusedasanargumentforthestyleparameterintheplotfunction.)

A named constant for the 'Step line with Breaks' style, to be used as an argument for the style parameter in the plot() function.
Type
const plot_style
See also
plot()
plot.style_line
plot.style_linebr
plot.style_histogram
plot.style_cross
plot.style_area
plot.style_areabr
plot.style_columns
plot.style_circles
plot.style_stepline
plot.style_stepline_diamond
position.bottom_center

## [Table position is used in table.new(), table.cell() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Tablepositionisusedintable.newtable.cellfunctions.)

Table position is used in table.new(), table.cell() functions.
Binds the table to the bottom edge in the center.
Type
const string
See also
table.new()
table.cell()
table.set_position()
position.top_left
position.top_center
position.top_right
position.middle_left
position.middle_center
position.middle_right
position.bottom_left
position.bottom_right
position.bottom_left

## [Table position is used in table.new(), table.cell() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Tablepositionisusedintable.newtable.cellfunctions.)

Table position is used in table.new(), table.cell() functions.
Binds the table to the bottom left of the screen.
Type
const string
See also
table.new()
table.cell()
table.set_position()
position.top_left
position.top_center
position.top_right
position.middle_left
position.middle_center
position.middle_right
position.bottom_center
position.bottom_right
position.bottom_right

## [Table position is used in table.new(), table.cell() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Tablepositionisusedintable.newtable.cellfunctions.)

Table position is used in table.new(), table.cell() functions.
Binds the table to the bottom right of the screen.
Type
const string
See also
table.new()
table.cell()
table.set_position()
position.top_left
position.top_center
position.top_right
position.middle_left
position.middle_center
position.middle_right
position.bottom_left
position.bottom_center
position.middle_center

## [Table position is used in table.new(), table.cell() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Tablepositionisusedintable.newtable.cellfunctions.)

Table position is used in table.new(), table.cell() functions.
Binds the table to the center of the screen.
Type
const string
See also
table.new()
table.cell()
table.set_position()
position.top_left
position.top_center
position.top_right
position.middle_left
position.middle_right
position.bottom_left
position.bottom_center
position.bottom_right
position.middle_left

## [Table position is used in table.new(), table.cell() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Tablepositionisusedintable.newtable.cellfunctions.)

Table position is used in table.new(), table.cell() functions.
Binds the table to the left side of the screen.
Type
const string
See also
table.new()
table.cell()
table.set_position()
position.top_left
position.top_center
position.top_right
position.middle_center
position.middle_right
position.bottom_left
position.bottom_center
position.bottom_right
position.middle_right

## [Table position is used in table.new(), table.cell() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Tablepositionisusedintable.newtable.cellfunctions.)

Table position is used in table.new(), table.cell() functions.
Binds the table to the right side of the screen.
Type
const string
See also
table.new()
table.cell()
table.set_position()
position.top_left
position.top_center
position.top_right
position.middle_left
position.middle_center
position.bottom_left
position.bottom_center
position.bottom_right
position.top_center

## [Table position is used in table.new(), table.cell() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Tablepositionisusedintable.newtable.cellfunctions.)

Table position is used in table.new(), table.cell() functions.
Binds the table to the top edge in the center.
Type
const string
See also
table.new()
table.cell()
table.set_position()
position.top_left
position.top_right
position.middle_left
position.middle_center
position.middle_right
position.bottom_left
position.bottom_center
position.bottom_right
position.top_left

## [Table position is used in table.new(), table.cell() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Tablepositionisusedintable.newtable.cellfunctions.)

Table position is used in table.new(), table.cell() functions.
Binds the table to the upper-left edge.
Type
const string
See also
table.new()
table.cell()
table.set_position()
position.top_center
position.top_right
position.middle_left
position.middle_center
position.middle_right
position.bottom_left
position.bottom_center
position.bottom_right
position.top_right

## [Table position is used in table.new(), table.cell() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Tablepositionisusedintable.newtable.cellfunctions.)

Table position is used in table.new(), table.cell() functions.
Binds the table to the upper-right edge.
Type
const string
See also
table.new()
table.cell()
table.set_position()
position.top_left
position.top_center
position.middle_left
position.middle_center
position.middle_right
position.bottom_left
position.bottom_center
position.bottom_right
scale.left

## [A named constant for use as the scale argument in indicator() and strategy() declaration statements. Specifies that the script's price scale is on the left side of the pane. If the script overlays on the main chart pane or another script's pane, it adds a new price scale on the left side of the pane and scales its visuals independently to fit the pane's visual space.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforuseasthescaleargumentinindicatorandstrategydeclarationstatements.Specifiesthatthescriptspricescaleisontheleftsideofthepane.Ifthescriptoverlaysonthemainchartpaneoranotherscriptspaneitaddsanewpricescaleontheleftsideofthepaneandscalesitsvisualsindependentlytofitthepanesvisualspace.)

A named constant for use as the scale argument in indicator() and strategy() declaration statements. Specifies that the script's price scale is on the left side of the pane. If the script overlays on the main chart pane or another script's pane, it adds a new price scale on the left side of the pane and scales its visuals independently to fit the pane's visual space.
Type
const scale_type
See also
indicator()
scale.none

## [A named constant for use as the scale argument in indicator() and strategy() declaration statements. A declaration statement can use this constant only if its overlay argument is true. Specifies that the script scales its visuals independently to fit the visual space of the main chart pane or another script's pane without displaying a separate scale. The script displays plotted numbers directly on the pane's existing price scale if the chart's settings allow it. If the user moves the script to a new pane, the script displays the values on a new scale to the left or right of that pane, depending on the chart's "Scales placement" setting.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforuseasthescaleargumentinindicatorandstrategydeclarationstatements.Adeclarationstatementcanusethisconstantonlyifitsoverlayargumentistrue.Specifiesthatthescriptscalesitsvisualsindependentlytofitthevisualspaceofthemainchartpaneoranotherscriptspanewithoutdisplayingaseparatescale.Thescriptdisplaysplottednumbersdirectlyonthepanesexistingpricescaleifthechartssettingsallowit.IftheusermovesthescripttoanewpanethescriptdisplaysthevaluesonanewscaletotheleftorrightofthatpanedependingonthechartsScalesplacementsetting.)

A named constant for use as the scale argument in indicator() and strategy() declaration statements. A declaration statement can use this constant only if its overlay argument is true. Specifies that the script scales its visuals independently to fit the visual space of the main chart pane or another script's pane without displaying a separate scale. The script displays plotted numbers directly on the pane's existing price scale if the chart's settings allow it. If the user moves the script to a new pane, the script displays the values on a new scale to the left or right of that pane, depending on the chart's "Scales placement" setting.
Type
const scale_type
See also
indicator()
scale.right

## [A named constant for use as the scale argument in indicator() and strategy() declaration statements. Specifies that the script's price scale is on the right side of the pane. If the script overlays on the main chart pane or another script's pane, it adds a new price scale on the right side of the pane and scales its visuals independently to fit the pane's visual space.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforuseasthescaleargumentinindicatorandstrategydeclarationstatements.Specifiesthatthescriptspricescaleisontherightsideofthepane.Ifthescriptoverlaysonthemainchartpaneoranotherscriptspaneitaddsanewpricescaleontherightsideofthepaneandscalesitsvisualsindependentlytofitthepanesvisualspace.)

A named constant for use as the scale argument in indicator() and strategy() declaration statements. Specifies that the script's price scale is on the right side of the pane. If the script overlays on the main chart pane or another script's pane, it adds a new price scale on the right side of the pane and scales its visuals independently to fit the pane's visual space.
Type
const scale_type
See also
indicator()
session.extended

## [Constant for extended session type (with extended hours data).](https://www.tradingview.com/pine-script-reference/v6/#const_Constantforextendedsessiontypewithextendedhoursdata.)

Constant for extended session type (with extended hours data).
Type
const string
See also
session.regular
syminfo.session
session.regular

## [Constant for regular session type (no extended hours data).](https://www.tradingview.com/pine-script-reference/v6/#const_Constantforregularsessiontypenoextendedhoursdata.)

Constant for regular session type (no extended hours data).
Type
const string
See also
session.extended
syminfo.session
settlement_as_close.inherit

## [A constant to specify the value of the settlement_as_close parameter in ticker.new() and ticker.modify() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Aconstanttospecifythevalueofthesettlement_as_closeparameterinticker.newandticker.modifyfunctions.)

A constant to specify the value of the settlement_as_close parameter in ticker.new() and ticker.modify() functions.
Type
const settlement
See also
ticker.new()
ticker.modify()
settlement_as_close.on
settlement_as_close.off
settlement_as_close.off

## [A constant to specify the value of the settlement_as_close parameter in ticker.new() and ticker.modify() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Aconstanttospecifythevalueofthesettlement_as_closeparameterinticker.newandticker.modifyfunctions.)

A constant to specify the value of the settlement_as_close parameter in ticker.new() and ticker.modify() functions.
Type
const settlement
See also
ticker.new()
ticker.modify()
settlement_as_close.on
settlement_as_close.inherit
settlement_as_close.on

## [A constant to specify the value of the settlement_as_close parameter in ticker.new() and ticker.modify() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Aconstanttospecifythevalueofthesettlement_as_closeparameterinticker.newandticker.modifyfunctions.)

A constant to specify the value of the settlement_as_close parameter in ticker.new() and ticker.modify() functions.
Type
const settlement
See also
ticker.new()
ticker.modify()
settlement_as_close.inherit
settlement_as_close.off
shape.arrowdown

## [Shape style for plotshape() function.](https://www.tradingview.com/pine-script-reference/v6/#const_Shapestyleforplotshapefunction.)

Shape style for plotshape() function.
Type
const string
See also
plotshape()
shape.arrowup

## [Shape style for plotshape() function.](https://www.tradingview.com/pine-script-reference/v6/#const_Shapestyleforplotshapefunction.)

Shape style for plotshape() function.
Type
const string
See also
plotshape()
shape.circle

## [Shape style for plotshape() function.](https://www.tradingview.com/pine-script-reference/v6/#const_Shapestyleforplotshapefunction.)

Shape style for plotshape() function.
Type
const string
See also
plotshape()
shape.cross

## [Shape style for plotshape() function.](https://www.tradingview.com/pine-script-reference/v6/#const_Shapestyleforplotshapefunction.)

Shape style for plotshape() function.
Type
const string
See also
plotshape()
shape.diamond

## [Shape style for plotshape() function.](https://www.tradingview.com/pine-script-reference/v6/#const_Shapestyleforplotshapefunction.)

Shape style for plotshape() function.
Type
const string
See also
plotshape()
shape.flag

## [Shape style for plotshape() function.](https://www.tradingview.com/pine-script-reference/v6/#const_Shapestyleforplotshapefunction.)

Shape style for plotshape() function.
Type
const string
See also
plotshape()
shape.labeldown

## [Shape style for plotshape() function.](https://www.tradingview.com/pine-script-reference/v6/#const_Shapestyleforplotshapefunction.)

Shape style for plotshape() function.
Type
const string
See also
plotshape()
shape.labelup

## [Shape style for plotshape() function.](https://www.tradingview.com/pine-script-reference/v6/#const_Shapestyleforplotshapefunction.)

Shape style for plotshape() function.
Type
const string
See also
plotshape()
shape.square

## [Shape style for plotshape() function.](https://www.tradingview.com/pine-script-reference/v6/#const_Shapestyleforplotshapefunction.)

Shape style for plotshape() function.
Type
const string
See also
plotshape()
shape.triangledown

## [Shape style for plotshape() function.](https://www.tradingview.com/pine-script-reference/v6/#const_Shapestyleforplotshapefunction.)

Shape style for plotshape() function.
Type
const string
See also
plotshape()
shape.triangleup

## [Shape style for plotshape() function.](https://www.tradingview.com/pine-script-reference/v6/#const_Shapestyleforplotshapefunction.)

Shape style for plotshape() function.
Type
const string
See also
plotshape()
shape.xcross

## [Shape style for plotshape() function.](https://www.tradingview.com/pine-script-reference/v6/#const_Shapestyleforplotshapefunction.)

Shape style for plotshape() function.
Type
const string
See also
plotshape()
size.auto

## [A constant to specify the size of the graphics drawn by plotchar(), plotshape(), label.new(), and box.new(). Adjusts the size of the graphics automatically.](https://www.tradingview.com/pine-script-reference/v6/#const_Aconstanttospecifythesizeofthegraphicsdrawnbyplotcharplotshapelabel.newandbox.new.Adjuststhesizeofthegraphicsautomatically.)

A constant to specify the size of the graphics drawn by plotchar(), plotshape(), label.new(), and box.new(). Adjusts the size of the graphics automatically.
Type
const string
See also
plotshape()
plotchar()
label.set_size()
size.tiny
size.small
size.normal
size.large
size.huge
size.huge

## [A constant to specify the size of the graphics drawn by plotchar(), plotshape(), label.new(), box.new(), and table.cell(). Sets the size to huge.](https://www.tradingview.com/pine-script-reference/v6/#const_Aconstanttospecifythesizeofthegraphicsdrawnbyplotcharplotshapelabel.newbox.newandtable.cell.Setsthesizetohuge.)

A constant to specify the size of the graphics drawn by plotchar(), plotshape(), label.new(), box.new(), and table.cell(). Sets the size to huge.
Type
const string
See also
plotshape()
plotchar()
label.set_size()
size.auto
size.tiny
size.small
size.normal
size.large
size.large

## [A constant to specify the size of the graphics drawn by plotchar(), plotshape(), label.new(), box.new(), and table.cell(). Sets the size to large.](https://www.tradingview.com/pine-script-reference/v6/#const_Aconstanttospecifythesizeofthegraphicsdrawnbyplotcharplotshapelabel.newbox.newandtable.cell.Setsthesizetolarge.)

A constant to specify the size of the graphics drawn by plotchar(), plotshape(), label.new(), box.new(), and table.cell(). Sets the size to large.
Type
const string
See also
plotshape()
plotchar()
label.set_size()
size.auto
size.tiny
size.small
size.normal
size.huge
size.normal

## [A constant to specify the size of the graphics drawn by plotchar(), plotshape(), label.new(), box.new(), and table.cell(). Sets the size to normal.](https://www.tradingview.com/pine-script-reference/v6/#const_Aconstanttospecifythesizeofthegraphicsdrawnbyplotcharplotshapelabel.newbox.newandtable.cell.Setsthesizetonormal.)

A constant to specify the size of the graphics drawn by plotchar(), plotshape(), label.new(), box.new(), and table.cell(). Sets the size to normal.
Type
const string
See also
plotshape()
plotchar()
label.set_size()
size.auto
size.tiny
size.small
size.large
size.huge
size.small

## [A constant to specify the size of the graphics drawn by plotchar(), plotshape(), label.new(), box.new(), and table.cell(). Sets the size to small.](https://www.tradingview.com/pine-script-reference/v6/#const_Aconstanttospecifythesizeofthegraphicsdrawnbyplotcharplotshapelabel.newbox.newandtable.cell.Setsthesizetosmall.)

A constant to specify the size of the graphics drawn by plotchar(), plotshape(), label.new(), box.new(), and table.cell(). Sets the size to small.
Type
const string
See also
plotshape()
plotchar()
label.set_size()
size.auto
size.tiny
size.normal
size.large
size.huge
size.tiny

## [A constant to specify the size of the graphics drawn by plotchar(), plotshape(), label.new(), box.new(), and table.cell(). Sets the size to tiny.](https://www.tradingview.com/pine-script-reference/v6/#const_Aconstanttospecifythesizeofthegraphicsdrawnbyplotcharplotshapelabel.newbox.newandtable.cell.Setsthesizetotiny.)

A constant to specify the size of the graphics drawn by plotchar(), plotshape(), label.new(), box.new(), and table.cell(). Sets the size to tiny.
Type
const string
See also
plotshape()
plotchar()
label.set_size()
size.auto
size.small
size.normal
size.large
size.huge
splits.denominator

## [A named constant for the request.splits() function. Is used to request the denominator (the number below the line in a fraction) of a splits.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantfortherequest.splitsfunction.Isusedtorequestthedenominatorthenumberbelowthelineinafractionofasplits.)

A named constant for the request.splits() function. Is used to request the denominator (the number below the line in a fraction) of a splits.
Type
const string
See also
request.splits()
splits.numerator

## [A named constant for the request.splits() function. Is used to request the numerator (the number above the line in a fraction) of a splits.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantfortherequest.splitsfunction.Isusedtorequestthenumeratorthenumberabovethelineinafractionofasplits.)

A named constant for the request.splits() function. Is used to request the numerator (the number above the line in a fraction) of a splits.
Type
const string
See also
request.splits()
strategy.cash

## [This is one of the arguments that can be supplied to the default_qty_type parameter in the strategy() declaration statement. It is only relevant when no value is used for the ‘qty’ parameter in strategy.entry() or strategy.order() function calls. It specifies that an amount of cash in the strategy.account_currency will be used to enter trades.](https://www.tradingview.com/pine-script-reference/v6/#const_Thisisoneoftheargumentsthatcanbesuppliedtothedefault_qty_typeparameterinthestrategydeclarationstatement.Itisonlyrelevantwhennovalueisusedfortheqtyparameterinstrategy.entryorstrategy.orderfunctioncalls.Itspecifiesthatanamountofcashinthestrategy.account_currencywillbeusedtoentertrades.)

This is one of the arguments that can be supplied to the default_qty_type parameter in the strategy() declaration statement. It is only relevant when no value is used for the ‘qty’ parameter in strategy.entry() or strategy.order() function calls. It specifies that an amount of cash in the strategy.account_currency will be used to enter trades.
Type
const string
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#const_version6)

//@version=6
strategy("strategy.cash", overlay = true, default_qty_value = 50, default_qty_type = strategy.cash, initial_capital = 1000000)

## [if bar_index == 0](https://www.tradingview.com/pine-script-reference/v6/#const_ifbar_index0)

if bar_index == 0
    // As ‘qty’ is not defined, the previously defined values for the `default_qty_type` and `default_qty_value` parameters are used to enter trades, namely 50 units of cash in the currency of `strategy.account_currency`.
    // `qty` is calculated as (default_qty_value)/(close price). If current price is $5, then qty = 50/5 = 10.
    strategy.entry("EN", strategy.long)
if bar_index == 2
    strategy.close("EN")
See also
strategy()
strategy.commission.cash_per_contract

## [Commission type for an order. Money displayed in the account currency per contract.](https://www.tradingview.com/pine-script-reference/v6/#const_Commissiontypeforanorder.Moneydisplayedintheaccountcurrencypercontract.)

Commission type for an order. Money displayed in the account currency per contract.
Type
const string
See also
strategy()
strategy.commission.cash_per_order

## [Commission type for an order. Money displayed in the account currency per order.](https://www.tradingview.com/pine-script-reference/v6/#const_Commissiontypeforanorder.Moneydisplayedintheaccountcurrencyperorder.)

Commission type for an order. Money displayed in the account currency per order.
Type
const string
See also
strategy()
strategy.commission.percent

## [Commission type for an order. A percentage of the cash volume of order.](https://www.tradingview.com/pine-script-reference/v6/#const_Commissiontypeforanorder.Apercentageofthecashvolumeoforder.)

Commission type for an order. A percentage of the cash volume of order.
Type
const string
See also
strategy()
strategy.direction.all

## [It allows strategy to open both long and short positions.](https://www.tradingview.com/pine-script-reference/v6/#const_Itallowsstrategytoopenbothlongandshortpositions.)

It allows strategy to open both long and short positions.
Type
const string
See also
strategy.risk.allow_entry_in()
strategy.direction.long

## [It allows strategy to open only long positions.](https://www.tradingview.com/pine-script-reference/v6/#const_Itallowsstrategytoopenonlylongpositions.)

It allows strategy to open only long positions.
Type
const string
See also
strategy.risk.allow_entry_in()
strategy.direction.short

## [It allows strategy to open only short positions.](https://www.tradingview.com/pine-script-reference/v6/#const_Itallowsstrategytoopenonlyshortpositions.)

It allows strategy to open only short positions.
Type
const string
See also
strategy.risk.allow_entry_in()
strategy.fixed

## [This is one of the arguments that can be supplied to the default_qty_type parameter in the strategy() declaration statement. It is only relevant when no value is used for the ‘qty’ parameter in strategy.entry() or strategy.order() function calls. It specifies that a number of contracts/shares/lots will be used to enter trades.](https://www.tradingview.com/pine-script-reference/v6/#const_Thisisoneoftheargumentsthatcanbesuppliedtothedefault_qty_typeparameterinthestrategydeclarationstatement.Itisonlyrelevantwhennovalueisusedfortheqtyparameterinstrategy.entryorstrategy.orderfunctioncalls.Itspecifiesthatanumberofcontractsshareslotswillbeusedtoentertrades.)

This is one of the arguments that can be supplied to the default_qty_type parameter in the strategy() declaration statement. It is only relevant when no value is used for the ‘qty’ parameter in strategy.entry() or strategy.order() function calls. It specifies that a number of contracts/shares/lots will be used to enter trades.
Type
const string
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#const_version6)

//@version=6
strategy("strategy.fixed", overlay = true, default_qty_value = 50, default_qty_type = strategy.fixed, initial_capital = 1000000)

## [if bar_index == 0](https://www.tradingview.com/pine-script-reference/v6/#const_ifbar_index0)

if bar_index == 0
    // As ‘qty’ is not defined, the previously defined values for the `default_qty_type` and `default_qty_value` parameters are used to enter trades, namely 50 contracts.
    // qty = 50
    strategy.entry("EN", strategy.long)
if bar_index == 2
    strategy.close("EN")
See also
strategy()
strategy.long

## [A named constant for use with the direction parameter of the strategy.entry() and strategy.order() commands. It specifies that the command creates a buy order.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforusewiththedirectionparameterofthestrategy.entryandstrategy.ordercommands.Itspecifiesthatthecommandcreatesabuyorder.)

A named constant for use with the direction parameter of the strategy.entry() and strategy.order() commands. It specifies that the command creates a buy order.
Type
const strategy_direction
See also
strategy.entry()
strategy.exit()
strategy.order()
strategy.oca.cancel

## [A named constant for use with the oca_type parameter of the strategy.entry() and strategy.order() commands. It specifies that the strategy cancels the unfilled order when another order with the same oca_name and oca_type executes.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforusewiththeoca_typeparameterofthestrategy.entryandstrategy.ordercommands.Itspecifiesthatthestrategycancelstheunfilledorderwhenanotherorderwiththesameoca_nameandoca_typeexecutes.)

A named constant for use with the oca_type parameter of the strategy.entry() and strategy.order() commands. It specifies that the strategy cancels the unfilled order when another order with the same oca_name and oca_type executes.
Type
const string
Remarks
Strategies cannot cancel or reduce pending orders from an OCA group if they execute on the same tick. For example, if the market price triggers two stop orders from strategy.order() calls with the same oca_* arguments, the strategy cannot fully or partially cancel either one.
See also
strategy.entry()
strategy.exit()
strategy.order()
strategy.oca.none

## [A named constant for use with the oca_type parameter of the strategy.entry() and strategy.order() commands. It specifies that the order executes independently of all other orders, including those with the same oca_name.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforusewiththeoca_typeparameterofthestrategy.entryandstrategy.ordercommands.Itspecifiesthattheorderexecutesindependentlyofallotherordersincludingthosewiththesameoca_name.)

A named constant for use with the oca_type parameter of the strategy.entry() and strategy.order() commands. It specifies that the order executes independently of all other orders, including those with the same oca_name.
Type
const string
See also
strategy.entry()
strategy.exit()
strategy.order()
strategy.oca.reduce

## [A named constant for use with the oca_type parameter of the strategy.entry() and strategy.order() commands. It specifies that when another order with the same oca_name and oca_type executes, the strategy reduces the unfilled order by that order's size. If the unfilled order's size reaches 0 after reduction, it is the same as canceling the order entirely.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforusewiththeoca_typeparameterofthestrategy.entryandstrategy.ordercommands.Itspecifiesthatwhenanotherorderwiththesameoca_nameandoca_typeexecutesthestrategyreducestheunfilledorderbythatorderssize.Iftheunfilledorderssizereaches0afterreductionitisthesameascancelingtheorderentirely.)

A named constant for use with the oca_type parameter of the strategy.entry() and strategy.order() commands. It specifies that when another order with the same oca_name and oca_type executes, the strategy reduces the unfilled order by that order's size. If the unfilled order's size reaches 0 after reduction, it is the same as canceling the order entirely.
Type
const string
Remarks
Strategies cannot cancel or reduce pending orders from an OCA group if they execute on the same tick. For example, if the market price triggers two stop orders from strategy.order() calls with the same oca_* arguments, the strategy cannot fully or partially cancel either one.
Orders from strategy.exit() automatically use this OCA type, and they belong to the same OCA group by default.
See also
strategy.entry()
strategy.exit()
strategy.order()
strategy.percent_of_equity

## [This is one of the arguments that can be supplied to the default_qty_type parameter in the strategy() declaration statement. It is only relevant when no value is used for the ‘qty’ parameter in strategy.entry() or strategy.order() function calls. It specifies that a percentage (0-100) of equity will be used to enter trades.](https://www.tradingview.com/pine-script-reference/v6/#const_Thisisoneoftheargumentsthatcanbesuppliedtothedefault_qty_typeparameterinthestrategydeclarationstatement.Itisonlyrelevantwhennovalueisusedfortheqtyparameterinstrategy.entryorstrategy.orderfunctioncalls.Itspecifiesthatapercentage0-100ofequitywillbeusedtoentertrades.)

This is one of the arguments that can be supplied to the default_qty_type parameter in the strategy() declaration statement. It is only relevant when no value is used for the ‘qty’ parameter in strategy.entry() or strategy.order() function calls. It specifies that a percentage (0-100) of equity will be used to enter trades.
Type
const string
Example

## [//@version=6](https://www.tradingview.com/pine-script-reference/v6/#const_version6)

//@version=6
strategy("strategy.percent_of_equity", overlay = false, default_qty_value = 100, default_qty_type = strategy.percent_of_equity, initial_capital = 1000000)

## [// As ‘qty’ is not defined, the previously defined values for the `default_qty_type` and `default_qty_value` parameters are used to enter trades, namely 100% of available equity.](https://www.tradingview.com/pine-script-reference/v6/#const_Asqtyisnotdefinedthepreviouslydefinedvaluesforthedefault_qty_typeanddefault_qty_valueparametersareusedtoentertradesnamely100ofavailableequity.)

// As ‘qty’ is not defined, the previously defined values for the `default_qty_type` and `default_qty_value` parameters are used to enter trades, namely 100% of available equity.
if bar_index == 0
    strategy.entry("EN", strategy.long)
if bar_index == 2
    strategy.close("EN")
plot(strategy.equity)

## [// The ‘qty’ parameter is set to 10. Entering position with fixed size of 10 contracts and entry market price = (10 * close).](https://www.tradingview.com/pine-script-reference/v6/#const_Theqtyparameterissetto10.Enteringpositionwithfixedsizeof10contractsandentrymarketprice10close.)

// The ‘qty’ parameter is set to 10. Entering position with fixed size of 10 contracts and entry market price = (10 * close).
if bar_index == 4
    strategy.entry("EN", strategy.long, qty = 10)
if bar_index == 6
    strategy.close("EN")
See also
strategy()
strategy.short

## [A named constant for use with the direction parameter of the strategy.entry() and strategy.order() commands. It specifies that the command creates a sell order.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforusewiththedirectionparameterofthestrategy.entryandstrategy.ordercommands.Itspecifiesthatthecommandcreatesasellorder.)

A named constant for use with the direction parameter of the strategy.entry() and strategy.order() commands. It specifies that the command creates a sell order.
Type
const strategy_direction
See also
strategy.entry()
strategy.exit()
strategy.order()
text.align_bottom

## [Vertical text alignment for box.new(), box.set_text_valign(), table.cell() and table.cell_set_text_valign() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Verticaltextalignmentforbox.newbox.set_text_valigntable.cellandtable.cell_set_text_valignfunctions.)

Vertical text alignment for box.new(), box.set_text_valign(), table.cell() and table.cell_set_text_valign() functions.
Type
const string
See also
table.cell()
table.cell_set_text_valign()
text.align_center
text.align_left
text.align_right
text.align_center

## [Text alignment for box.new(), box.set_text_halign(), box.set_text_valign(), label.new() and label.set_textalign() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Textalignmentforbox.newbox.set_text_halignbox.set_text_valignlabel.newandlabel.set_textalignfunctions.)

Text alignment for box.new(), box.set_text_halign(), box.set_text_valign(), label.new() and label.set_textalign() functions.
Type
const string
See also
label.new()
label.set_style()
text.align_left
text.align_right
text.align_left

## [Horizontal text alignment for box.new(), box.set_text_halign(), label.new() and label.set_textalign() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Horizontaltextalignmentforbox.newbox.set_text_halignlabel.newandlabel.set_textalignfunctions.)

Horizontal text alignment for box.new(), box.set_text_halign(), label.new() and label.set_textalign() functions.
Type
const string
See also
label.new()
label.set_style()
text.align_center
text.align_right
text.align_right

## [Horizontal text alignment for box.new(), box.set_text_halign(), label.new() and label.set_textalign() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Horizontaltextalignmentforbox.newbox.set_text_halignlabel.newandlabel.set_textalignfunctions.)

Horizontal text alignment for box.new(), box.set_text_halign(), label.new() and label.set_textalign() functions.
Type
const string
See also
label.new()
label.set_style()
text.align_center
text.align_left
text.align_top

## [Vertical text alignment for box.new(), box.set_text_valign(), table.cell() and table.cell_set_text_valign() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Verticaltextalignmentforbox.newbox.set_text_valigntable.cellandtable.cell_set_text_valignfunctions.)

Vertical text alignment for box.new(), box.set_text_valign(), table.cell() and table.cell_set_text_valign() functions.
Type
const string
See also
table.cell()
table.cell_set_text_valign()
text.align_center
text.align_left
text.align_right
text.format_bold

## [A named constant for use with the text_formatting parameter of the label.new(), box.new(), table.cell(), and *set_text_formatting() functions. Makes the text bold.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforusewiththetext_formattingparameterofthelabel.newbox.newtable.cellandset_text_formattingfunctions.Makesthetextbold.)

A named constant for use with the text_formatting parameter of the label.new(), box.new(), table.cell(), and *set_text_formatting() functions. Makes the text bold.
Type
const text_format
See also
label.new()
box.new()
table.cell()
text.format_italic

## [A named constant for use with the text_formatting parameter of the label.new(), box.new(), table.cell(), and *set_text_formatting() functions. Italicizes the text.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforusewiththetext_formattingparameterofthelabel.newbox.newtable.cellandset_text_formattingfunctions.Italicizesthetext.)

A named constant for use with the text_formatting parameter of the label.new(), box.new(), table.cell(), and *set_text_formatting() functions. Italicizes the text.
Type
const text_format
See also
label.new()
box.new()
table.cell()
text.format_none

## [A named constant for use with the text_formatting parameter of the label.new(), box.new(), table.cell(), and *set_text_formatting() functions. Signifies no special text formatting.](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantforusewiththetext_formattingparameterofthelabel.newbox.newtable.cellandset_text_formattingfunctions.Signifiesnospecialtextformatting.)

A named constant for use with the text_formatting parameter of the label.new(), box.new(), table.cell(), and *set_text_formatting() functions. Signifies no special text formatting.
Type
const text_format
See also
label.new()
box.new()
table.cell()
text.wrap_auto

## [Automatic wrapping mode for box.new() and box.set_text_wrap() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Automaticwrappingmodeforbox.newandbox.set_text_wrapfunctions.)

Automatic wrapping mode for box.new() and box.set_text_wrap() functions.
Type
const string
See also
box.new()
box.set_text()
box.set_text_wrap()
text.wrap_none

## [Disabled wrapping mode for box.new() and box.set_text_wrap() functions.](https://www.tradingview.com/pine-script-reference/v6/#const_Disabledwrappingmodeforbox.newandbox.set_text_wrapfunctions.)

Disabled wrapping mode for box.new() and box.set_text_wrap() functions.
Type
const string
See also
box.new()
box.set_text()
box.set_text_wrap()
true

## [Literal representing one of the values a bool variable can hold, or an expression can evaluate to when it uses comparison or logical operators.](https://www.tradingview.com/pine-script-reference/v6/#const_Literalrepresentingoneofthevaluesaboolvariablecanholdoranexpressioncanevaluatetowhenitusescomparisonorlogicaloperators.)

Literal representing one of the values a bool variable can hold, or an expression can evaluate to when it uses comparison or logical operators.
Remarks
See the User Manual for comparison operators and logical operators.
See also
bool
xloc.bar_index

## [A constant that specifies how functions that create and modify Pine drawings interpret x-coordinates. If xloc = xloc.bar_index, the drawing object treats each x-coordinate as a bar_index value.](https://www.tradingview.com/pine-script-reference/v6/#const_AconstantthatspecifieshowfunctionsthatcreateandmodifyPinedrawingsinterpretx-coordinates.Ifxlocxloc.bar_indexthedrawingobjecttreatseachx-coordinateasabar_indexvalue.)

A constant that specifies how functions that create and modify Pine drawings interpret x-coordinates. If xloc = xloc.bar_index, the drawing object treats each x-coordinate as a bar_index value.
Type
const string
See also
xloc.bar_time
line.new()
label.new()
box.new()
polyline.new()
line.set_xloc()
label.set_xloc()
xloc.bar_time

## [A constant that specifies how functions that create and modify Pine drawings interpret x-coordinates. If xloc = xloc.bar_time, the drawing object treats each x-coordinate as a UNIX timestamp, expressed in milliseconds.](https://www.tradingview.com/pine-script-reference/v6/#const_AconstantthatspecifieshowfunctionsthatcreateandmodifyPinedrawingsinterpretx-coordinates.Ifxlocxloc.bar_timethedrawingobjecttreatseachx-coordinateasaUNIXtimestampexpressedinmilliseconds.)

A constant that specifies how functions that create and modify Pine drawings interpret x-coordinates. If xloc = xloc.bar_time, the drawing object treats each x-coordinate as a UNIX timestamp, expressed in milliseconds.
Type
const string
See also
xloc.bar_index
line.new()
label.new()
box.new()
polyline.new()
line.set_xloc()
label.set_xloc()
xloc.bar_index
yloc.abovebar

## [A named constant that specifies the algorithm of interpretation of y-value in function label.new().](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantthatspecifiesthealgorithmofinterpretationofy-valueinfunctionlabel.new.)

A named constant that specifies the algorithm of interpretation of y-value in function label.new().
Type
const string
See also
label.new()
label.set_yloc()
yloc.price
yloc.belowbar
yloc.belowbar

## [A named constant that specifies the algorithm of interpretation of y-value in function label.new().](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantthatspecifiesthealgorithmofinterpretationofy-valueinfunctionlabel.new.)

A named constant that specifies the algorithm of interpretation of y-value in function label.new().
Type
const string
See also
label.new()
label.set_yloc()
yloc.price
yloc.abovebar
yloc.price

## [A named constant that specifies the algorithm of interpretation of y-value in function label.new().](https://www.tradingview.com/pine-script-reference/v6/#const_Anamedconstantthatspecifiesthealgorithmofinterpretationofy-valueinfunctionlabel.new.)

A named constant that specifies the algorithm of interpretation of y-value in function label.new().
Type
const string
See also
label.new()
label.set_yloc()
yloc.abovebar
yloc.belowbar
