# add-led-brightless-to-graph-lcd-3d-printer
add led brightless to reprap discount graph lcd 64x128 3d printer
/*add this part in pins.h line:770*/
 #ifdef REPRAP_DISCOUNT_SMART_CONTROLLER
    #define BEEPER -1 
    #define BTN_EN1 33  
    #define BTN_EN2 31 
    #define BTN_ENC 35
    #define LED_PIN 5 //added by tuna for led brightless
 /*add this part in ultralcd.h line:18*/
 #ifdef DOGLCD
  extern int lcd_contrast;
  extern int led_brightless; //added by tuna for led brightless
  void lcd_setcontrast(uint8_t value);
#endif
/*add this part in ConfigurationStore.h line:89*/
  #ifndef DOGLCD
    int lcd_contrast = 32;
    int led_brightless = DEFAULT_LED_BRIGHTLESS;  //added by tuna for led brightless
  #endif
  EEPROM_WRITE_VAR(i,lcd_contrast);
  EEPROM_WRITE_VAR(i,led_brightless);  //added by tuna for led brightless
/*continue line:325*/
  #ifndef DOGLCD
     int lcd_contrast;
     int led_brightless;  //added by tuna for led brightless
  #endif
     EEPROM_READ_VAR(i,lcd_contrast);
     EEPROM_READ_VAR(i,led_brightless);  //added by tuna for led brightless
 /*continue line:419*/
 #ifdef DOGLCD
    lcd_contrast = DEFAULT_LCD_CONTRAST;
    led_brightless = DEFAULT_LED_BRIGHTLESS; //added by tuna for led brightless
#endif
/*add this part in language_en.h line:94*/
#define MSG_CONTRAST              "LCD contrast"
#define MSG_LED_CONTRAST					"LED brightless"  // added by tuna for led brightless
/*add this part in dogm_lcd_implementation.h line:75 */
int lcd_contrast;
int led_brightless;//added by tuna for led brigtless
/*continue line:98 */
    u8g.setContrast(lcd_contrast);	
    u8g.setContrast(led_brightless); //added by tuna for led brigtless
/*add this part in ultralcd.cpp line:65*/
    #ifdef DOGLCD
    static void lcd_set_contrast();
    static void lcd_control_led(); //added by tuna for led brightless
/*continue line:751*/
    #ifdef DOGLCD
      //MENU_ITEM_EDIT(int3, MSG_CONTRAST, &lcd_contrast, 0, 63);
      MENU_ITEM(submenu, MSG_CONTRAST, lcd_set_contrast);
      MENU_ITEM(submenu, MSG_LED_CONTRAST, lcd_control_led); //added by tuna for led brightless
    #endif
/*continue line:901*/
#ifdef DOGLCD
static void lcd_set_contrast()
{
    if (encoderPosition != 0)
    {
        lcd_contrast -= encoderPosition;
        if (lcd_contrast < 0) lcd_contrast = 0;
        else if (lcd_contrast > 63) lcd_contrast = 63;
        encoderPosition = 0;
        lcdDrawUpdate = 1;
        u8g.setContrast(lcd_contrast);
    }
    if (lcdDrawUpdate)
    {
        lcd_implementation_drawedit(PSTR(MSG_CONTRAST), itostr2(lcd_contrast));
    }
    if (LCD_CLICKED) lcd_goto_menu(lcd_control_menu);

}
/*add bythis function for led brightless settings 
---------------------add by tuna----------------*/
static void lcd_control_led()
{

   if (encoderPosition != 0)
   {
        led_brightless = led_brightless - encoderPosition*2;
        if (led_brightless < 0) led_brightless = 0;
        else if (led_brightless > 255) led_brightless = 255;
        encoderPosition = 0;
        lcdDrawUpdate = 1;
        u8g.setContrast(encoderPosition);
   }

    if (lcdDrawUpdate)
    {
        lcd_implementation_drawedit(PSTR("Brightless"), itostr3(led_brightless));
    }

    if (LCD_CLICKED) lcd_goto_menu(lcd_control_menu);


    analogWrite(LED_PIN,led_brightless); //LED_PIN 5 in defined pins.h

}
/*finished led brightless function*/
#endif
/*add this part in Configuration.h line:702*/
      #ifdef DOGLCD
      #define DEFAULT_LED_BRIGHTLESS 150 //added by tuna for led brightless
      #ifndef DEFAULT_LCD_CONTRAST
        #define DEFAULT_LCD_CONTRAST 32

      #endif
      #endif
