# NFC-Reader
Follow the follwing steps to read nfc chip.

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
		
