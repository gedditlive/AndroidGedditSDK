# AndroidGedditSDK
This is the way how one can integrate our SDK.


Integration Process of Our Android-SDK.

We have very easy to integrate SDK released on popular distribution network Bintray. For a client to integrate our SDK in their current Android Application, they require to follow following steps.
1. The client need to include following two dependencies in their app.gradle file.

implementation 'com.geddit.live.sdk:core:1.0.14'(Version may vary)
implementation 'com.geddit.live.sdk:commerce:1.0.21'(Version may vary)

2. Next they need to initialise our SDK at the point where client wish to using our Builder class providing essential information about the client’s app and user details.

fun startGedditSDK(context: Context, view: View){

    val sharePrefHelper = SharePrefHelper(context)
    val userIdPref = sharePrefHelper.getString(
        VerifyActivity.USER_ID_PREF_KEY,
        "5cb830cd117cc61866a25fca") ?: ""
    val namePref = sharePrefHelper.getString(
        VerifyActivity.NAME_PREF_KEY,
        "Chihiro") ?: ""
    val emailPref = sharePrefHelper.getString(
        VerifyActivity.EMAIL_PREF_KEY,
        "snuxker@mail.com") ?: ""
    val mobileNumberPref = sharePrefHelper.getString(
        VerifyActivity.MOBILE_PREF_KEY,
        "+66894587114") ?: ""
    val imageUrlPref = sharePrefHelper.getString(
        VerifyActivity.IMAGE_URL_PREF_KEY,
        "https://www.prapagorn.com/images/avatar.jpg") ?: ""
    val appIdPref = BuildConfig.APP_ID
    val appSecretPref = BuildConfig.APP_SECRET
    val geddit = GedditLiveCommerce.Builder(
        context,
        appIdPref,
        appSecretPref,
        userIdPref,
        namePref,
        "THB",
        object: GedditLiveCommerce.GedditLiveCommerceListener{

            override fun onCollectionImageClicked(context: Context, product: Product) {
                GedditHelper.helper?.enterPIP()
                startProductDetail(context, product)
            }

            override fun onCollectionAddToBagClicked(context: Context, product: Product) {
                showSizeSelection(
                    context,
                    view,
                    product
                )
            }

            override fun onReady(helper:  GedditLiveCommerce.GedditLiveCommerceActionListener) {
                GedditHelper.helper = helper
            }
        },
        object: VoucherDelegate {
            override fun didWinVoucher(voucher: Voucher) {
                Log.d("Voucher_delegate","Voucher id"+voucher.voucherId+" Voucher Code "+voucher.voucherCode+" Voucher Description "+voucher.voucherDescription)
            }
        },
        object : QuizDelegate {
            override fun didAnswerCorrectlyQuestion(question: String, answer: String) {
                Log.d("didAnswerCorrectly","question "+question+" answer "+answer)

            }
            override fun didAnswerIncorrectlyQuestion(question: String, answer: String) {
                Log.d("didAnswerIncorrectly","question "+question+" answer "+answer)

            }
        }
        )
        .email(emailPref)
        .imageUrl(imageUrlPref)
        .gender(Gender.MALE) // Gender.MALE, Gender.FEMALE, Gender.OTHER
        .mobileNumber(mobileNumberPref)
        .languageCode("en")
        .fcmToken("")
        .debuggable(BuildConfig.DEBUG)

     GedditLiveCommerce.with(geddit.build())
}

3. We have 4 important delegates which allows client to receive events when the actions are performed in our SDK.
      1.  when user interacts with the product listed on a show:
        
  interface GedditLiveCommerceListener {   
    fun onReady(helper: GedditLiveCommerceActionListener)
    fun onCollectionImageClicked(context: Context, product: Product)
    fun onCollectionAddToBagClicked(context: Context, product: Product)
 }
    
   
      2. When user plays trivia :
      
         
interface QuizDelegate {
    fun didAnswerCorrectlyQuestion(question: String, answer: String)
    fun didAnswerIncorrectlyQuestion(question: String, answer: String)
}

     
3. when user win a voucher:


interface VoucherDelegate: Serializable {
    fun didWinVoucher(voucher: Voucher)
}

4. Picture in Picture Mode delegate:


interface GedditLiveCommerceActionListener: Serializable {
    fun enterPIP()
    fun exitPIP()
    fun exitPIP(product: Product)
}

Requirements:


 Android Studio 4.0.1 or above
 Gradle  gradle-6.1.1-all.zip or above
 Kotlin 1.3.50 or above

Now Client can start our SDK and use the features in their existing App.
