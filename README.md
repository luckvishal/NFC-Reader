# NFC-Reader

#Place these line in activity in onCreate----------

 mNfcAdapter = NfcAdapter.getDefaultAdapter(this);
        if (mNfcAdapter == null) {
            Toast.makeText(this, "Your device doesn't support NFC", Toast.LENGTH_SHORT).show();
            finish();
            return;
        }
        pendingIntent = PendingIntent.getActivity(this, 0, new Intent(this, getClass()).addFlags(536870912), 0);
        IntentFilter ndef = new IntentFilter("android.nfc.action.NDEF_DISCOVERED");
        try{
            ndef.addDataType("*/*");
            ndef.addCategory("android.intent.category.DEFAULT");
            IntentFilter ndef2 = new IntentFilter("android.nfc.action.NDEF_DISCOVERED");
            ndef2.addDataScheme("http");
            ndef2.addDataScheme("https");
            ndef2.addCategory("android.intent.category.DEFAULT");
            IntentFilter tech = new IntentFilter("android.nfc.action.TECH_DISCOVERED");
            tech.addCategory("android.intent.category.DEFAULT");
            IntentFilter tag = new IntentFilter("android.nfc.action.TAG_DISCOVERED");
            tech.addCategory("android.intent.category.DEFAULT");
            intentFiltersArray = new IntentFilter[]{ndef, ndef2, tech, tag};
            String[][] strArr = new String[7][];
            strArr[0] = new String[]{NfcV.class.getName()};
            strArr[1] = new String[]{NfcA.class.getName()};
            strArr[2] = new String[]{NfcB.class.getName()};
            strArr[3] = new String[]{IsoDep.class.getName()};
            strArr[4] = new String[]{NdefFormatable.class.getName()};
            strArr[5] = new String[]{MifareClassic.class.getName()};
            strArr[6] = new String[]{MifareUltralight.class.getName()};
            techListsArray = strArr;
        } catch (IntentFilter.MalformedMimeTypeException e) {
            throw new RuntimeException("fail", e);
        }
-------------------------------------------------------------------------------------------
#this function call when any one Tap NFC tag-------------

public void onNewIntent(Intent intent) {
        HelperTagReader helperTagReader = new HelperTagReader();
        if(NetworkManager.isInternetConnected(LoginActivity.this)) {
            if (loginCount == 0) {
                if ("android.nfc.action.TECH_DISCOVERED".equals(intent.getAction())) {
                    // HelperTagReader.handleIntent(intent, LoginActivity.this);
                    loginCount =1;
                    finalRead();

                } else if ("android.nfc.action.NDEF_DISCOVERED".equals(intent.getAction())) {
                    loginCount =1;
                    finalRead();

                } else if ("android.nfc.action.TAG_DISCOVERED".equals(intent.getAction())) {
                    loginCount =1;
                    finalRead();

                }
            } else
                return;
        }
        else if(networkPopupDisplayed){
            networkPopupDisplayed = false;
            showInternetSettingsAlert(LoginActivity.this);
        }
    }



#and call this method from Reader button or Activity-------

finalRead(){
 Parcelable[] rawMsgs = intent.getParcelableArrayExtra(NfcAdapter.EXTRA_NDEF_MESSAGES);
        NdefMessage[] msgs;

        byte[] mimeBytes = "application/com.android.cts.verifier.nfc"
                .getBytes(Charset.forName("US-ASCII"));
        //byte[] id = new byte[] {1, 3, 3, 7};
        //byte[] payload = "CTS Verifier NDEF Push Tag".getBytes(Charset.forName("US-ASCII"));

        byte[] empty = new byte[0];
        byte[] id = intent.getByteArrayExtra(NfcAdapter.EXTRA_ID);
        Parcelable tag = intent.getParcelableExtra(NfcAdapter.EXTRA_TAG);
        byte[] payload = dumpTagData(tag).getBytes(Charset.forName("US-ASCII"));

        NdefRecord record = new NdefRecord(NdefRecord.TNF_UNKNOWN, empty, id, payload);
        NdefMessage msg = new NdefMessage(new NdefRecord[] { record });
        msgs = new NdefMessage[] { msg };
        tid = msgs[0].getRecords()[0].toString().split("id=")[1].split(" payload=")[0];
        pid= msgs[0].getRecords()[0].toString().split("id=")[1].split(" payload=")[1];

        if (rawMsgs != null) {
            msgs = new NdefMessage[rawMsgs.length];
            for (int i = 0; i < rawMsgs.length; i++) {
                msgs[i] = (NdefMessage) rawMsgs[i];
            }

        } else {
            // Unknown tag type

        }
        
        }
