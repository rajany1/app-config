# free_stylemakers

A new Flutter project.

## Getting Started

This project is a starting point for a Flutter application.

Frist maa ta ma yo app ma online game ko custom room banayera payers lai room ma khelayera earn garauna chahanchu
jstai 1 match ma 48 players hunchan. Ex: 1 player ko entry fee Rs 25 huncha, 48 players ma 19 jana winner hunchan, jastai
    Total payers (Tp) = 48 players
    Total number of winner players (Twp) = (40% of Total Players) 19 players
    Winning price for each winner (except top 1) = Rs 50 (double the entry fee)
    Top 1 position player earns = Rs 100 (4 times the entry fee, i.e., Rs 100)


But Top 1 positions player get Rs 100 (300% more than the entry fee).


Program structure
1. First Screen
   1. flash screen
2. login and signup screen  
   1. first of all login ra signup
3. Game ko UID ra Name Paste half Screen from bottom
4. Home screen
   1. App Bar
      1. logo, balance, UID, setting
         1. Setting
            1. setting ma `Need Help?`
               1. How to use this app?
            2. `Contact Us` ma messanger ra `follow us` (in fb)
            3. share the app
   2. Body
      1. Options button
         1. Full Map matches/Clash Squared matches
            1. Solo Matches card
               1. Free fire. Maximum 48/4 players
                  1. solo/duo/squared category
                     1. Winners out of 48/4 players 
                        1. Colum(|)
                           1. Top 19/top 12
                           2. 2.0x/3.0x
                           3. Choose Time (5:00 pm)
                           4. Entry fee: Rs 25 (Button)
   3. Footer app bar
      1. Home
      2. Redeem
      3. Earn money from ads

landing screen finished.




matches (Collection)
│── Free Fire (Document)
│   ├── gameMatches (Subcollection)
│   │   ├── Match1 (Document)
│   │   │   ├── title: "Solo Matches"
│   │   │   ├── players: 48
│   │   │   ├── time: Timestamp (e.g., "2025-03-24 17:00:00 UTC")
│   │   │   ├── entryFee: 25
│   │   │   ├── remainingSeats: 10
│   │   │   ├── roomId: "ABC123" // थपिएको
│   │   │   ├── password: "xyz789" // थपिएको
│   │   ├── Match2 (Document)
│── PUBG (Document)
│   ├── gameMatches (Subcollection)
│   │   ├── Match1 (Document)
│   │   │   ├── title: "Squad Match"
│   │   │   ├── players: "100 Players"
│   │   │   ├── time: "7:00 PM"
│   │   │   ├── entryFee: "100"
│   │   │   ├── remainingSeats: "10"
│   │   │   ├── roomId: "ABC123" // थपिएको
│   │   │   ├── password: "xyz789" // थपिएको
│── BGMI (Document)
│   ├── gameMatches (Subcollection)
│   │   ├── Match1 (Document)
│   │   │   ├── title: "Duo Match"
│   │   │   ├── players: "50 Teams"
│   │   │   ├── time: "8:00 PM"
│   │   │   ├── entryFee: "75"
│   │   │   ├── remainingSeats: "10"
│   │   │   ├── roomId: "ABC123" // थपिएको
│   │   │   ├── password: "xyz789" // थपिएको

profiles (Collection)
│── userId (Document)
│   ├── balance: 500
│   ├── gameName: "Player123"
│   ├── joinedMatches: ["Match1", "Match2"] // Array of match IDs


1. HomeScreen ko card
   1. 'Click' Entry fee - If UID and Amount then give error message, else -> Show Details of Match 
      1. Free Fire - type (solo, squared, Duo, clashsquard, etc).
      2. Entry Fee - 
      3. Winners players - 
      4. winning Amount - 
      5. Show message for players - If there are 48 players in the match: -1st prize: 100. -2nd to 19th prize: 50
      6. match time - 01:20
      7. Map type: Random Maps (Bermuda, Purgatory)
      8. See massage for players.
      9. Thank You 




Maile k Chahanchu vanda User ko balance 1, ads bata earn huncha. 2, load garesi earn huncha. 3. game jitesi haldiyera load huncha
aaba ghatni kura chai 1. game ma entry garesi ghatcha temporary. 2, withdrawal request garesi ghatcha aani 24 hours ma money withdrawal vayena vane cancel huncha ra return aaucha
   feri request garna paucha user le, with error/reason.
Ani user le privacy and policy bigarcha vane teslai penalti dincha aani yeuta `news and announcement` vanincha warning pani dincha.



watchreferralspin (collection)
└── watchreferralspinId (document)
├── userId (field) : string // Unique identifier for the user
├── earnedCoins (field) : integer // Total earned coins by the user
├── spinCount (field) : integer // Remaining spins count for the day
├── watchCount (field) : integer // Remaining watch ads count for the day
├── referralCode (field) : string // User's unique referral code
├── referredBy (field) : string // Referral code of the user who referred this user (null if not referred)
├── completedTasks (field) : boolean // Whether the tasks are completed today
├── lastTaskReset (field) : timestamp // Time when tasks were last reset
├── lastSpinTime (field) : timestamp // Timestamp of the last spin activity (optional)
├── lastAdWatchedTime (field) : timestamp // Timestamp of the last ad watched (optional)
├── referralEarnings (field) : integer // Coins earned from referrals
├── referrals (subcollection)
│    └── referralId (document)
│         ├── referredUserId (field) : string // ID of the user referred
│         ├── referralDate (field) : timestamp // Date when the user was referred
│         ├── referralStatus (field) : string // Status of referral (e.g., "pending", "completed")
│         ├── totalEarnings (field) : integer // Total earnings of this referred user
│         ├── referrerEarnings (field) : integer // 1% of totalEarnings given to the referrer
└── adViews (subcollection) (optional)
└── adViewId (document)
├── adType (field) : string // Type of ad viewed (e.g., "rewarded", "interstitial")
├── viewTime (field) : timestamp // Timestamp when the ad was viewed
├── rewardEarned (field) : integer // Coins earned from viewing the ad




import firebase_admin
from firebase_admin import credentials, firestore
import time
import re
import subprocess

# Firebase सेटअप
cred = credentials.Certificate("path/to/your-service-account.json")
firebase_admin.initialize_app(cred)
db = firestore.client()

# तपाईंको PSP ID हरू
PSP_IDS = {
"eSewa": "your-esewa-id",
"Khalti": "your-khalti-id",
"IMEpay": "your-imepay-id"
}

def read_sms(payment_method):
cmd = "termux-sms-list -l 10"  # पछिल्लो १० SMS पढ्छ
result = subprocess.run(cmd, shell=True, capture_output=True, text=True)
sms_list = result.stdout

    if payment_method == "eSewa":
        pattern = r"Received Rs\. (\d+\.?\d*) from .* Transaction ID: (\w+)"
    elif payment_method == "Khalti":
        pattern = r"Rs\. (\d+\.?\d*) received from .* Transaction ID: (\w+)"
    elif payment_method == "IMEpay":
        pattern = r"Received Rs (\d+\.?\d*) .* Transaction ID: (\w+)"
    else:
        return None, None

    match = re.search(pattern, sms_list)
    if match:
        amount = float(match.group(1))
        transaction_id = match.group(2)
        return amount, transaction_id
    return None, None

def verify_deposits():
deposits_ref = db.collection('deposits').where('status', '==', 'pending')
docs = deposits_ref.stream()

    for doc in docs:
        data = doc.to_dict()
        payment_method = data['paymentMethod']
        expected_amount = data['amount']
        transaction_code = data['transactionCode']
        psp_id = data['pspId']

        received_amount, received_transaction_id = read_sms(payment_method)

        if (received_amount == expected_amount and 
            received_transaction_id == transaction_code and 
            psp_id == PSP_IDS[payment_method]):
            doc.reference.update({
                'status': 'success',
                'timestamp': firestore.SERVER_TIMESTAMP
            })
            print(f"Verified {payment_method} deposit of Rs {expected_amount}")
        else:
            print(f"Verification failed for {payment_method} deposit")

while True:
verify_deposits()
time.sleep(10)  # हरेक १० सेकेन्डमा जाँच#   a p p - c o n f i g  
 