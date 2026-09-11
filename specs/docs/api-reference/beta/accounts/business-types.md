> ## Documentation Index
> Fetch the complete documentation index at: https://docs.whop.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Business Types and Industries

> The business_type, industry_group, and industry_type values that classify an account.

Three fields classify accounts: `business_type`, `industry_group`, and `industry_type`.

Dropdown structure:

```text theme={null}
business_type
    industry_group
        industry_type
```

Set them on [Create Account](/api-reference/beta/accounts/create-account) or [Update Account](/api-reference/beta/accounts/update-account); read them back on the [Account object](/api-reference/beta/accounts/account).

<AccordionGroup>
  <Accordion title="education">
    <AccordionGroup>
      <Accordion title="trading_and_investing">
        `trading_general`, `technical_analysis`, `options_alerts`,
        `crypto_signals`, `futures_signals`, `ict_smc_trading`,
        `futures_trading`, `stock_signals`, `forex_signals`, `options_trading`,
        `forex_trading`, `day_trading`, `crypto_trading`, `prop_firm_trading`,
        `nft_alpha`, `stock_trading`, `algorithmic_trading`,
        `long_term_investing`, `alternative_investments`, `swing_trading`,
        `penny_stock_trading`, `prediction_markets`, `value_investing`,
        `index_fund_investing`, `forex_scalping`, `gold_precious_metals`,
        `private_equity`, `macro_economics`, `personalized_investment_advice`,
        `dividend_investing`, `venture_capital`
      </Accordion>

      <Accordion title="sports_betting_and_gambling">
        `sports_picks`, `prop_bets`, `soccer_picks`, `fantasy_sports`,
        `horse_racing`, `mma_picks`, `mlb_picks`, `nfl_picks`, `nba_picks`,
        `esports_picks`, `poker`, `sports_analytics`
      </Accordion>

      <Accordion title="business_and_entrepreneurship">
        `ecommerce`, `business_strategy`, `reselling`, `dropshipping`,
        `coaching_business`, `agency_building`, `retail_arbitrage`,
        `local_business`, `amazon_fba`, `make_money_online`, `wholesaling`,
        `startups`, `consulting_business`, `ticket_reselling`, `merch_business`,
        `womens_entrepreneurship`, `private_label`, `saas`, `etsy`,
        `freelancing`, `cleaning_business`, `business_acquisition`,
        `executives`, `vending_machine_business`, `print_on_demand`,
        `trucking_business`, `car_wash_business`, `atm_business`,
        `solopreneurship`, `licensing_business`
      </Accordion>

      <Accordion title="sales">
        `high_ticket_sales`, `appointment_setting`, `b2b_sales`,
        `sales_funnels`, `insurance_sales`, `sales_general`,
        `door_to_door_sales`, `retail_sales`, `car_sales`, `solar_sales`
      </Accordion>

      <Accordion title="marketing">
        `digital_marketing`, `affiliate_marketing`, `tiktok_marketing`,
        `facebook_ads`, `instagram_growth`, `smma`, `ai_marketing`,
        `email_marketing`, `copywriting`, `seo`, `google_ads`,
        `webinar_marketing`, `saas_marketing`, `local_seo`, `youtube_marketing`,
        `event_marketing`
      </Accordion>

      <Accordion title="creative_and_content_creation">
        `influencer`, `ai_content_creation`, `clipping`, `youtube_automation`,
        `video_editing`, `content_creation_general`, `ugc_creation`,
        `illustration`, `design_general`, `photography`, `ui_ux_design`,
        `3d_modeling`, `youtube_creator`, `fashion_design`, `writing`,
        `filmmaking`, `wedding_photography`, `calligraphy_lettering`,
        `interior_design`, `podcasting`
      </Accordion>

      <Accordion title="real_estate">
        `real_estate_wholesaling`, `airbnb_str`, `rental_property`,
        `property_management`, `house_flipping`, `land_investing`,
        `section_8_housing`, `multifamily_investing`, `commercial_real_estate`,
        `real_estate_investing`, `property_development`, `real_estate_general`,
        `mobile_home_investing`, `vacation_rental_management`,
        `self_storage_investing`
      </Accordion>

      <Accordion title="digital_products_and_publishing">
        `digital_product_creation`, `amazon_kdp`, `course_creation`,
        `self_publishing`, `blogging`, `templates`, `ai_book_publishing`,
        `audiobook_publishing`, `ghostwriting`
      </Accordion>

      <Accordion title="fitness_and_sports_training">
        `body_recomposition`, `bodybuilding`, `weight_loss`,
        `athletic_performance`, `flexibility_mobility`, `fitness_general`,
        `strength_training`, `calisthenics`, `soccer`, `nutrition`, `running`,
        `martial_arts`, `golf`, `boxing`, `basketball`, `yoga`, `pilates`,
        `tennis`, `womens_fitness`, `swimming`, `racket_sports`, `jiu_jitsu`,
        `mma`, `cycling`, `gymnastics`, `wrestling`, `crossfit`,
        `postpartum_fitness`
      </Accordion>

      <Accordion title="health_and_wellness">
        `holistic_health`, `biohacking`, `mental_health`, `stress_management`,
        `womens_health`, `addiction_recovery`, `meditation_mindfulness`,
        `gut_health`, `mens_health`, `trauma_recovery`, `biomarker_health`,
        `adhd`, `breathwork`, `fertility`, `longevity`, `grief_recovery`,
        `chronic_illness`
      </Accordion>

      <Accordion title="personal_development">
        `personal_development_general`, `mindset`, `appearance_and_grooming`,
        `mens_self_improvement`, `productivity`, `networking`,
        `womens_self_improvement`, `life_coaching`, `public_speaking`,
        `leadership_development`, `neurolinguistic_programming`,
        `anger_management`, `stoicism`
      </Accordion>

      <Accordion title="spirituality_and_faith">
        `spirituality_general`, `christianity`, `manifestation`,
        `faith_general`, `energy_healing`, `islam`, `astrology`, `numerology`,
        `psychic_development`, `chakra_healing`, `shamanic_healing`
      </Accordion>

      <Accordion title="dating_and_relationships">
        `mens_dating`, `relationships`, `breakup_recovery`, `marriage`,
        `dating_general`, `womens_dating`, `communication_skills`,
        `masculinity`, `femininity`
      </Accordion>

      <Accordion title="personal_finance">
        `wealth_building`, `budgeting`, `credit_repair`, `tax_strategy`,
        `credit_card_optimization`, `student_loans`, `personal_finance_general`
      </Accordion>

      <Accordion title="career_and_professional_skills">
        `career_development`, `trade_skills`, `tech_careers`, `law`,
        `personal_branding`, `healthcare_careers`, `executive_coaching`,
        `virtual_assistant`, `management_skills`, `bookkeeping`, `data_careers`,
        `finance_careers`, `consulting_careers`, `law_careers`,
        `teaching_careers`, `human_resources`
      </Accordion>

      <Accordion title="tech_and_ai">
        `web_development`, `ai_agents`, `prompt_engineering`, `ai_general`,
        `cybersecurity`, `no_code`, `game_development`,
        `software_development_general`, `automation`, `data_science`,
        `cloud_computing`, `blockchain`, `python`, `javascript`,
        `data_engineering`, `databases`, `linux_sysadmin`, `indie_hacking`,
        `robotics`, `wordpress`, `product_management`, `fintech`, `vr_ar`,
        `devops`, `open_source`, `tech_industry`, `climate_tech`
      </Accordion>

      <Accordion title="academic_and_test_prep">
        `tutoring`, `college_admissions`, `medical_board_prep`,
        `graduate_school_prep`, `stem_education`, `professional_certifications`,
        `homeschooling`, `ap_exam_prep`, `bar_exam_prep`, `scholarships`
      </Accordion>

      <Accordion title="languages">
        `english`, `arabic`, `spanish`, `languages_general`, `mandarin`,
        `japanese`, `french`, `accent_reduction`, `business_english`, `german`,
        `korean`, `sign_language`
      </Accordion>

      <Accordion title="lifestyle_and_culture">
        `fashion`, `books`, `sports_fandom`
      </Accordion>

      <Accordion title="collecting_and_enthusiasts">
        `collectibles`, `sneakers`, `cars`, `watches`, `cigars`
      </Accordion>

      <Accordion title="food_and_drink">
        `cooking`, `baking`, `mixology`, `wine`, `homebrewing`
      </Accordion>

      <Accordion title="home_family_and_pets">
        `parenting`, `pets`, `gardening`, `homesteading`, `alternative_living`,
        `aquarium_fishkeeping`, `home_design`
      </Accordion>

      <Accordion title="travel_and_outdoors">
        `travel`, `survival_prepping`, `expat_living`, `astronomy`, `boating`,
        `bird_watching`, `hunting`, `motorcycle_riding`, `scuba_diving`,
        `rock_climbing`, `skiing_snowboarding`, `surfing`, `fishing`
      </Accordion>

      <Accordion title="crafts_and_making">
        `diy_crafts`, `knitting_crocheting`, `jewelry_making`, `woodworking`,
        `magic`, `floral_design`, `pottery_ceramics`
      </Accordion>

      <Accordion title="music_and_performing_arts">
        `music_theory`, `music_production`, `music_business`, `dance`, `acting`,
        `djing`, `voice_acting`
      </Accordion>

      <Accordion title="gaming_and_esports">
        `gaming`, `game_coaching`, `esports`
      </Accordion>

      <Accordion title="news_and_general_interest">
        `science_and_medicine`, `entertainment`, `politics`, `geopolitics`,
        `philosophy`, `education_sector`, `journalism`, `defense_and_security`,
        `law_and_policy`, `sustainability`, `architecture`, `history`,
        `psychology`
      </Accordion>

      <Accordion title="regulated_and_prohibited">
        `adult_content`, `political_and_charitable_fundraising`,
        `gambling_and_sweepstakes`, `counterfeit_and_piracy`,
        `hate_and_violence`
      </Accordion>
    </AccordionGroup>
  </Accordion>

  <Accordion title="ecommerce">
    <AccordionGroup>
      <Accordion title="clothing_and_apparel">
        `everyday_clothing`, `streetwear`, `luxury_fashion`,
        `outerwear_jackets`, `athleisure`, `custom_apparel`, `vintage_clothing`,
        `lingerie_intimates`, `swimwear`, `plus_size_fashion`,
        `sleepwear_loungewear`, `denim_brand`, `workwear`, `socks_hosiery`,
        `kids_clothing`, `maternity_clothing`, `costumes_cosplay`,
        `dance_performance_wear`, `scrubs_medical_apparel`,
        `hunting_camo_apparel`
      </Accordion>

      <Accordion title="accessories_and_jewelry">
        `jewelry`, `bags_wallets`, `keychains_charms`, `hats_headwear`,
        `sunglasses_eyewear`, `phone_accessories`, `travel_accessories`,
        `custom_engraved_accessories`, `tech_accessories`, `belts`,
        `scarves_wraps`, `hair_accessories`
      </Accordion>

      <Accordion title="beauty_and_personal_care">
        `skincare`, `haircare`, `cosmetics_makeup`, `body_care`, `fragrance`,
        `intimate_care`, `oral_care`, `hair_growth_products`, `mens_grooming`,
        `lip_care`, `sunscreen_spf`, `deodorant`, `acne_treatment`,
        `baby_skincare`, `tattoo_aftercare`
      </Accordion>

      <Accordion title="supplements_and_nutrition">
        `herbal_supplements`, `dietary_supplements`, `pet_supplements`,
        `protein_supplements`, `weight_management_supplements`,
        `collagen_supplements`, `kids_supplements`, `nootropics`, `gut_health`,
        `nutraceutical_products`, `vitamins_minerals`, `testosterone_boosters`,
        `sleep_supplements`, `joint_bone_health`, `ayurvedic_supplements`,
        `creatine_supplements`, `mushroom_supplements`, `greens_powder`,
        `pre_workout`, `immune_support`, `electrolyte_hydration`,
        `prenatal_supplements`, `keto_supplements`
      </Accordion>

      <Accordion title="home_and_living">
        `home_decor`, `lighting_fixtures`, `kitchenware`, `bedding_linens`,
        `cleaning_products`, `reusable_products`, `smart_home`,
        `wall_art_prints`, `organization_storage`, `luxury_home_goods`,
        `outdoor_furniture`, `rugs_carpets`, `planters_garden_decor`,
        `home_fragrance`, `bathroom_accessories`, `seasonal_holiday_decor`,
        `solar_powered_products`
      </Accordion>

      <Accordion title="food_and_beverages">
        `meal_kits`, `snacks_treats`, `baked_goods`, `beverages`,
        `specialty_coffee_tea`, `sauces_condiments`, `health_food`,
        `pet_food_treats`, `subscription_food_box`, `jerky_meat_snacks`,
        `honey_sweeteners`, `olive_oil_vinegar`, `plant_based_food`,
        `keto_food_products`, `kombucha_fermented`, `protein_bars_snacks`,
        `chocolate_confections`, `hot_sauce`, `dried_fruit_nuts`, `baby_food`,
        `gluten_free_food`
      </Accordion>

      <Accordion title="fitness_equipment_and_gear">
        `recovery_equipment`, `home_gym_equipment`, `yoga_equipment`,
        `outdoor_fitness_gear`, `wearable_fitness`, `weightlifting_equipment`,
        `posture_correctors`, `cardio_equipment`, `combat_sports_gear`,
        `gymnastics_equipment`, `swimming_gear`, `jump_rope_equipment`,
        `grip_strength_tools`, `sauna_cold_plunge`
      </Accordion>

      <Accordion title="outdoor_and_sports">
        `camping_hiking`, `cycling_gear`, `fishing_gear`, `golf_equipment`,
        `tennis_equipment`, `pickleball_equipment`, `tactical_gear`,
        `hunting_gear`, `water_sports_gear`, `equestrian_gear`,
        `snow_sports_gear`, `climbing_gear`, `archery_equipment`,
        `skateboarding_gear`, `overlanding_gear`
      </Accordion>

      <Accordion title="electronics_and_gadgets">
        `portable_tech`, `audio_equipment`, `gaming_hardware`,
        `camera_equipment`, `charging_power`, `smart_wearables`,
        `drones_robotics`, `projectors_displays`, `streaming_devices`,
        `home_security_devices`, `3d_printers`, `vr_headsets`, `e_readers`,
        `hardware_wallets`
      </Accordion>

      <Accordion title="baby_and_kids">
        `kids_toys`, `kids_educational_products`, `baby_products`, `kids_books`,
        `baby_clothing`, `baby_safety_products`, `nursery_decor`,
        `kids_outdoor_play`, `kids_arts_crafts`
      </Accordion>

      <Accordion title="pets_and_animals">
        `dog_products`, `cat_products`, `pet_grooming_products`,
        `horse_supplies`, `pet_apparel`, `pet_tech`, `pet_home_products`,
        `aquarium_supplies`, `bird_supplies`, `reptile_supplies`
      </Accordion>

      <Accordion title="arts_and_crafts">
        `stationery`, `craft_kits`, `sewing_textiles`, `pottery_supplies`,
        `scrapbooking_supplies`, `beading_jewelry_supplies`,
        `printmaking_supplies`
      </Accordion>

      <Accordion title="automotive">
        `car_accessories`, `performance_parts`, `car_care_products`,
        `motorcycle_gear`, `off_road_parts`, `car_audio_electronics`,
        `ev_charging_accessories`, `truck_accessories`
      </Accordion>

      <Accordion title="tools_office_and_supplies">
        `painting_and_building_supplies`, `office_supplies`,
        `power_tools_and_accessories`, `desk_accessories`, `shipping_packaging`,
        `hardware_and_fasteners`, `workshop_equipment_and_storage`,
        `hand_tools`, `safety_and_work_gear`, `printing_supplies`
      </Accordion>

      <Accordion title="religion_and_faith">
        `christian_books`, `christian_jewelry`, `other_religious_goods`,
        `islamic_prayer_goods`, `christian_apparel`, `christian_home_decor`,
        `islamic_apparel`, `buddhist_meditation_goods`, `jewish_judaica`,
        `jewish_books`, `jewish_apparel`, `islamic_books`,
        `hindu_puja_supplies`, `hindu_books`, `buddhist_books`,
        `sikh_religious_goods`
      </Accordion>

      <Accordion title="regulated_and_prohibited">
        `drugs_and_controlled_substances`, `high_risk_goods`,
        `counterfeit_and_piracy`, `alcohol_and_tobacco`, `cbd_products`,
        `weapons`
      </Accordion>
    </AccordionGroup>
  </Accordion>

  <Accordion title="platform">
    <AccordionGroup>
      <Accordion title="digital_goods_marketplaces">
        `courses`, `plugin_theme`, `stock_media`, `templates`, `music_beats`,
        `ebooks`, `3d_models`, `code_snippets`, `nfts`, `prompts`
      </Accordion>

      <Accordion title="services_and_freelance_marketplaces">
        `creative_services`, `freelance`, `coaching`, `home_services`,
        `healthcare`, `dj_entertainment`, `auto_services`, `beauty_services`,
        `fitness_trainer`, `tutoring`, `elder_care`, `legal_services`,
        `wedding_services`, `pet_services`, `childcare`, `translation`,
        `therapy`, `photography`
      </Accordion>

      <Accordion title="b2b_and_saas_marketplaces">
        `saas`, `agencies`, `commercial_real_estate`, `manufacturing`,
        `logistics`, `business_for_sale`
      </Accordion>

      <Accordion title="physical_goods_marketplaces">
        `collectibles`, `vintage_resale`, `luxury_goods`, `wholesale`,
        `event_tickets`, `handmade_goods`, `electronics`, `auto_parts`,
        `local_goods`, `pet_goods`, `dropshipping`, `sneakers`, `books`,
        `furniture`, `musical_instrument`, `art`, `industrial_equipment`,
        `craft_supply`, `baby_kids`, `outdoor_gear`, `sustainable_goods`,
        `supplements`
      </Accordion>

      <Accordion title="rental_marketplaces">
        `vehicle_rental`, `equipment_rental`, `vacation_rental`,
        `rv_camper_rental`, `space_rental`, `clothing_rental`,
        `camera_gear_rental`, `boat_rental`, `storage_rental`,
        `office_coworking_rental`, `parking_rental`
      </Accordion>

      <Accordion title="travel_food_and_hospitality_marketplaces">
        `travel_planning`, `restaurant`, `grocery`, `airline_tickets`,
        `cruise_bookings`, `hotel_bookings`, `catering`, `homemade_food`,
        `meal_prep`, `bakery`, `farm_produce`, `chef_booking`
      </Accordion>

      <Accordion title="affiliate_and_ad_networks">
        `affiliate_marketing`
      </Accordion>

      <Accordion title="fintech_and_payment_platforms">
        `payment_facilitation`, `prop_trading`, `lending`, `crowdfunding`,
        `crypto_exchange`, `investment_advice`, `prediction_markets`,
        `token_issuance`, `tipping`, `yield_products`
      </Accordion>

      <Accordion title="regulated_and_prohibited">
        `counterfeit_and_piracy`, `drugs_and_controlled_substances`,
        `high_risk_goods`, `weapons`, `alcohol_and_tobacco`,
        `gambling_and_sweepstakes`, `unlicensed_and_high_risk_services`,
        `fraud_and_deception`
      </Accordion>
    </AccordionGroup>
  </Accordion>

  <Accordion title="software">
    <AccordionGroup>
      <Accordion title="trading_and_investing_tools">
        `trading_indicators`, `futures_trading_bot`, `forex_trading_bot`,
        `stock_research`, `options_flow`, `portfolio_tracker`,
        `crypto_trading_bot`, `backtesting`, `crypto_trading`,
        `market_data_feed`, `prop_trading`, `stock_trading`
      </Accordion>

      <Accordion title="fintech_and_payments">
        `accounting`, `payment_facilitation`, `financial_modeling`,
        `risk_management`, `crypto_wallets`, `invoicing`, `banking`, `tax`,
        `crypto_exchange`, `prediction_markets`, `token_issuance`, `lending`,
        `insurance`, `bnpl`, `check_cashing`, `consumer_lending`,
        `credit_repair`, `debt_collection`, `debt_relief`, `escrow`,
        `foreign_exchange`, `yield_products`, `tipping`
      </Accordion>

      <Accordion title="ecommerce_and_reselling_tools">
        `resale_arbitrage`, `ecommerce_storefronts`, `reseller_management`,
        `checkout_optimization`, `product_research`, `print_on_demand`,
        `marketplace_seller`, `price_tracker`, `shipping`,
        `product_feed_management`, `wholesale_ordering`, `returns_management`,
        `product_reviews`
      </Accordion>

      <Accordion title="marketing_and_sales_software">
        `ad_management`, `lead_generation`, `link_in_bio`, `crm`,
        `email_marketing`, `analytics_dashboard`, `seo`, `ai_social_media`,
        `influencer_marketing`, `landing_page_builder`, `ai_outreach`,
        `affiliate_tracking`, `competitive_intelligence`, `whatsapp_marketing`,
        `sms_marketing`, `video_sales`, `social_listening`, `ai_sales`,
        `review_management`, `ab_testing`, `proposals`
      </Accordion>

      <Accordion title="ai_and_automation">
        `ai_chatbot`, `workflow_automation`, `ai_writing`, `generative_ai`,
        `ai_agents`, `llm_api`, `ai_phone_agent`
      </Accordion>

      <Accordion title="productivity_and_collaboration">
        `task_management`, `project_management`, `document_collaboration`,
        `note_taking`, `ai_data_analysis`, `contract_management`, `ai_research`,
        `knowledge_base`, `form_survey_builder`, `scheduling`,
        `expense_management`, `okr_goal_tracking`, `ai_meeting_assistant`,
        `ai_presentation`, `asset_management`, `ai_translation`,
        `facility_management`, `visitor_management`
      </Accordion>

      <Accordion title="customer_support_and_communication">
        `customer_messaging`, `ai_customer_support`, `business_phone_system`,
        `team_communication`, `video_conferencing`
      </Accordion>

      <Accordion title="hr_and_people_software">
        `human_resources`, `time_tracking`, `applicant_tracking`,
        `ai_recruiting`, `employee_engagement`, `onboarding`
      </Accordion>

      <Accordion title="developer_and_infrastructure_tools">
        `no_code_builder`, `hosting`, `api_management`, `code_editor`, `devops`,
        `testing`, `ai_code_assistant`, `monitoring`, `documentation`,
        `databases`, `cdn`, `error_tracking`, `webhooks`
      </Accordion>

      <Accordion title="security_and_privacy_software">
        `vpn`, `people_search`, `cybersecurity`, `endpoint_protection`,
        `data_privacy`, `password_manager`, `email_security`, `backup_recovery`,
        `access_management`, `compliance`, `identity_verification`
      </Accordion>

      <Accordion title="creative_and_media_software">
        `ai_video`, `animation`, `video_editing`, `ai_image_generator`,
        `photo_editing`, `music_production`, `streaming`, `ai_voice`,
        `audio_editing`, `ai_music`, `screen_recording`
      </Accordion>

      <Accordion title="gaming_software">
        `game_mods`, `game_server_hosting`
      </Accordion>

      <Accordion title="betting_and_gambling_software">
        `sports_betting`, `loot_boxes`, `skill_contests_paid_entry`,
        `unlicensed_gambling`, `skill_contests_free_entry`,
        `sweepstakes_raffles`, `fantasy_sports_free_to_play`,
        `fantasy_sports_paid_entry`, `gambling_operations`
      </Accordion>

      <Accordion title="health_and_wellness_software">
        `wellness`, `nutrition_tracking`, `fitness`, `mental_health`,
        `patient_engagement`, `practice_management`, `health_data`, `ehr`,
        `medical_billing`, `pharmacy_management`, `lab_management`,
        `dental_practice`, `ai_healthcare`, `telehealth`, `clinical_trials`,
        `veterinary_practice`
      </Accordion>

      <Accordion title="real_estate_software">
        `deal_analysis`, `property_management`, `real_estate_crm`,
        `real_estate_marketing`, `virtual_tours`, `mls_search`,
        `home_valuation`, `construction_management`
      </Accordion>

      <Accordion title="industry_specific_software">
        `field_service`, `restaurant_pos`, `auto_shop`, `logistics`,
        `legal_practice`, `agriculture`, `gym_management`, `salon`,
        `nonprofit_management`, `hotel_pms`, `church_management`,
        `cleaning_business`, `marketplace_management`, `childcare_management`,
        `roofing`, `landscaping`, `ai_legal`, `marina_management`,
        `pest_control`, `tattoo_studio`
      </Accordion>

      <Accordion title="digital_goods_and_accounts">
        `license_key_reselling`, `account_sharing`, `account_generation`
      </Accordion>

      <Accordion title="community_education_and_events_software">
        `virtual_classroom`, `community`, `newsletter`, `event_management`,
        `webinar`, `podcast_hosting`, `school_management`, `forums`,
        `primary_event_ticketing`, `ticket_marketplace`
      </Accordion>

      <Accordion title="regulated_and_prohibited">
        `counterfeit_and_piracy`, `adult_content`, `fraud_and_deception`,
        `drugs_and_controlled_substances`, `weapons`,
        `unlicensed_and_high_risk_services`
      </Accordion>
    </AccordionGroup>
  </Accordion>

  <Accordion title="other">
    <AccordionGroup>
      <Accordion title="miscellaneous">
        `other_general`, `media_company`, `niche_service`, `hybrid_business`,
        `niche_product`, `holding_company`, `family_office`, `cooperative`,
        `social_enterprise`, `incubator_accelerator`, `coworking_community`,
        `research_lab`
      </Accordion>

      <Accordion title="nonprofit_and_charity">
        `personal_fundraising`, `unregistered_charities`,
        `nonprofit_organization`, `community_organization`, `youth_nonprofit`,
        `religious_organization`, `environmental_nonprofit`,
        `animal_welfare_nonprofit`, `disaster_relief_nonprofit`,
        `charity_foundation`, `education_nonprofit`, `health_nonprofit`,
        `arts_culture_nonprofit`, `social_justice_nonprofit`,
        `veterans_nonprofit`, `food_bank`, `housing_nonprofit`,
        `registered_501c3`
      </Accordion>

      <Accordion title="government_politics_and_public">
        `political_campaign`, `government_agency`, `public_utility`,
        `public_library`, `public_school`, `municipal_service`,
        `military_installation`, `embassy_consulate`, `political_fundraising`,
        `political_organizations`
      </Accordion>
    </AccordionGroup>
  </Accordion>

  <Accordion title="brick_and_mortar">
    <AccordionGroup>
      <Accordion title="hospitality_and_lodging">
        `vacation_rental`, `hotel`, `bed_and_breakfast`, `retreat_center`,
        `campground`
      </Accordion>

      <Accordion title="automotive">
        `car_wash`, `auto_repair_shop`, `auto_body_shop`, `car_dealership`,
        `auto_parts_store`, `ev_charging_station`
      </Accordion>

      <Accordion title="fitness_and_recreation">
        `gym`, `martial_arts_gym`, `sports_facility`, `fitness_studio`,
        `shooting_range`, `equestrian_center`, `golf_course`
      </Accordion>

      <Accordion title="beauty_and_wellness">
        `hair_salon`, `spa`, `med_spa`, `lash_brow_and_wax_studio`,
        `barbershop`, `tattoo_parlor`, `tanning_salon`, `nail_salon`,
        `beauty_supply_store`
      </Accordion>

      <Accordion title="healthcare_clinics">
        `dental_office`, `medical_clinic`, `physical_therapy_clinic`,
        `chiropractic_office`, `optometry_office`, `pharmacy`,
        `veterinary_clinic`, `mental_health_clinic`, `acupuncture_clinic`
      </Accordion>

      <Accordion title="entertainment_and_leisure">
        `live_venue`, `attraction`, `family_entertainment_center`,
        `nightlife_venue`
      </Accordion>

      <Accordion title="retail_stores">
        `clothing_and_fashion_store`, `books_music_and_hobby_store`,
        `home_and_garden_store`, `optical_store`, `electronics_store`,
        `pet_store`, `sporting_goods_store`, `grocery_and_convenience_store`
      </Accordion>

      <Accordion title="restaurants_bars_and_cafes">
        `cafe_and_bakery`, `restaurant`, `quick_service_food`, `catering`,
        `butcher_shop`, `bar_and_lounge`, `brewery_winery_and_distillery`
      </Accordion>

      <Accordion title="home_and_trade_showrooms">
        `hvac_and_plumbing_showroom`, `kitchen_and_bath_showroom`,
        `pool_spa_showroom`, `solar_showroom`,
        `window_door_and_fireplace_showroom`
      </Accordion>

      <Accordion title="professional_services_offices">
        `laundry_and_dry_cleaning`, `storage_facility`, `real_estate_office`,
        `financial_office`, `coworking_space`, `law_office`, `insurance_office`,
        `print_and_shipping_center`, `travel_agency_storefront`,
        `staffing_office`, `immigration_office`
      </Accordion>

      <Accordion title="education_and_childcare">
        `preschool_and_daycare`, `arts_school`, `tutoring_center`,
        `driving_school`, `language_school`, `vocational_school`, `swim_school`
      </Accordion>

      <Accordion title="pet_services">
        `dog_training_facility`, `pet_grooming`, `pet_boarding_and_daycare`
      </Accordion>

      <Accordion title="other_local_businesses">
        `farm_supply`, `funeral_home`, `farm`, `biohazard_cleanup`,
        `estate_liquidation`
      </Accordion>

      <Accordion title="regulated_and_prohibited">
        `drugs_and_controlled_substances`, `alcohol_and_tobacco`, `weapons`,
        `unlicensed_and_high_risk_services`
      </Accordion>
    </AccordionGroup>
  </Accordion>

  <Accordion title="services">
    <AccordionGroup>
      <Accordion title="marketing_and_advertising">
        `performance_marketing`, `social_media_marketing`, `local_marketing`,
        `seo`, `growth_marketing`, `dental_marketing`, `email_marketing`,
        `ecommerce_marketing`, `content_marketing`, `influencer_marketing`,
        `real_estate_marketing`, `tiktok_marketing`, `public_relations`,
        `b2b_marketing`, `conversion_optimization`, `amazon_marketing`,
        `event_marketing`, `affiliate_management`, `video_marketing`,
        `restaurant_marketing`, `linkedin_marketing`, `podcast_marketing`
      </Accordion>

      <Accordion title="sales_and_lead_generation">
        `appointment_setting`, `lead_generation`, `outsourced_sales`,
        `cold_outreach`, `crm_implementation`, `revenue_operations`,
        `sales_training`, `door_to_door_sales`, `outbound_telemarketing`
      </Accordion>

      <Accordion title="consulting">
        `done_for_you`, `management_consulting`, `operations_consulting`,
        `it_consulting`, `brand_strategy_consulting`,
        `saas_marketing_consulting`, `digital_transformation_consulting`,
        `real_estate_consulting`, `hr_consulting`, `m_and_a_consulting`,
        `education_consulting`, `export_trade_consulting`,
        `nonprofit_consulting`, `compliance_consulting`,
        `supply_chain_consulting`, `change_management_consulting`,
        `healthcare_consulting`, `restaurant_consulting`,
        `sustainability_consulting`, `franchise_consulting`,
        `pricing_strategy_consulting`, `legal_consulting`
      </Accordion>

      <Accordion title="creative_and_media_production">
        `video_production`, `video_clipping`, `web_design`, `graphic_design`,
        `branding`, `content_writing`, `ugc`, `photography`, `music_production`,
        `motion_design`, `ghostwriting`, `3d_visualization`, `ui_ux_design`,
        `scriptwriting`, `event_production`, `product_design`,
        `podcast_production`, `voice_over`, `fashion_design`, `translation`,
        `drone_services`, `illustration`
      </Accordion>

      <Accordion title="software_development_and_it">
        `web_development`, `devops`, `software_development`,
        `ecommerce_development`, `mobile_development`, `api_integration`,
        `wordpress`, `game_development`, `cybersecurity`, `ai_development`,
        `blockchain_development`, `data_engineering`, `vr_ar_development`
      </Accordion>

      <Accordion title="ai_and_automation_agencies">
        `ai_automation`, `workflow_automation`, `ai_voice_agent`, `ai_chatbot`,
        `ai_consulting`, `machine_learning`, `ai_content`, `data_analytics`,
        `computer_vision`
      </Accordion>

      <Accordion title="professional_services">
        `legal`, `real_estate`, `business_formation`, `government_facilitation`,
        `tax_services`, `accounting_bookkeeping`, `notary`,
        `property_management`, `appraisal`, `immigration`,
        `class_action_administration`, `audit`, `mediation`,
        `intellectual_property`, `payroll`, `forensic_accounting`, `actuarial`
      </Accordion>

      <Accordion title="financial_services">
        `financial_consulting`, `financial_planning`, `prop_firm_passing`,
        `managed_trading`, `credit_repair`, `insurance_brokerage`,
        `mortgage_brokerage`, `lending`, `escrow`, `check_cashing`,
        `crowdfunding`, `crypto_brokerage`, `debt_collection`, `debt_relief`,
        `foreign_exchange`, `payment_facilitation`, `investment_advice`,
        `prediction_markets`, `token_issuance`, `tipping`, `yield_products`
      </Accordion>

      <Accordion title="recruiting_and_staffing">
        `staffing`, `virtual_assistant_staffing`, `recruiting`,
        `executive_recruiting`, `event_staffing`
      </Accordion>

      <Accordion title="personal_and_lifestyle_services">
        `concierge`, `tutoring`, `personal_assistant`, `travel_planning`,
        `personal_styling`, `wedding_planning`, `gig_work`, `pet_care`,
        `airline_tickets`, `home_organizing`, `personal_chef`, `errands`,
        `elder_care`, `cruise_bookings`, `childcare`, `relocation`,
        `hotel_bookings`, `personal_shopping`, `laundry`, `car_detailing`,
        `tour_guide`
      </Accordion>

      <Accordion title="health_and_wellness_services">
        `personal_training`, `nutrition`, `lab_testing`, `counseling`, `medspa`,
        `massage_therapy`, `acupuncture`, `addiction_recovery`, `chiropractic`,
        `doula_and_midwifery`, `lactation_consulting`, `dietitian`, `iv_therapy`
      </Accordion>

      <Accordion title="telehealth_and_medical">
        `functional_and_integrative_care`, `weight_and_metabolic_care`,
        `mental_health_care`, `rehabilitation_therapy`, `womens_health_care`,
        `primary_care`, `chronic_and_sleep_care`, `specialty_care`,
        `prescription_delivery`, `online_pharmacy`, `mens_health_care`,
        `veterinary_care`
      </Accordion>

      <Accordion title="home_and_trade_services">
        `pressure_washing`, `cleaning`, `handyman`, `landscaping`,
        `junk_removal`, `plumbing`, `home_renovation`, `electrical`, `hvac`,
        `roofing`, `window_cleaning`, `moving`, `concrete_masonry`,
        `insulation`, `pest_control`, `tree_service`, `epoxy_coating`,
        `painting`, `pool_service`, `home_inspection`, `septic`,
        `waterproofing`, `locksmith`, `glass_window`, `solar_installation`,
        `garage_door`, `gutter`, `flooring`, `cabinet_countertop`, `chimney`,
        `fencing`, `snow_removal`
      </Accordion>

      <Accordion title="logistics_and_transportation">
        `freight_brokerage`, `courier`, `auto_transport`, `warehousing`,
        `delivery`, `international_shipping`, `rideshare`, `chauffeur`,
        `cold_chain_logistics`, `vehicle_rental`
      </Accordion>

      <Accordion title="industrial_and_manufacturing">
        `3d_printing`, `metal_fabrication`, `industrial_automation_integrator`,
        `oil_and_gas`, `cnc_machining`, `waste_management_recycling`,
        `contract_manufacturing`, `plastic_injection_molding`, `pcba_assembly`,
        `chemical_manufacturing`, `textile_manufacturing`,
        `food_processing_facility`, `packaging_manufacturing`,
        `mining_and_extraction`, `renewable_energy_generation`,
        `hazardous_waste_disposal`, `aerospace_defense_contracting`
      </Accordion>

      <Accordion title="security_and_investigations">
        `security_guards`, `private_investigation`, `armored_transport`,
        `security_systems`
      </Accordion>

      <Accordion title="media_and_entertainment_companies">
        `talent_management`, `book_publishing_house`, `magazine_publisher`,
        `record_label`, `radio_broadcasting`, `film_studio`, `music_licensing`,
        `advertising_network`, `live_entertainment`, `news_media_outlet`,
        `tv_production_company`, `ad_tech_platform`
      </Accordion>

      <Accordion title="customer_support_and_bpo">
        `technical_support`, `outsourced_support`, `community_management`,
        `inbound_teleservices`, `call_center`
      </Accordion>

      <Accordion title="regulated_and_prohibited">
        `unlicensed_and_high_risk_services`, `fraud_and_deception`,
        `adult_content`, `drugs_and_controlled_substances`,
        `bounty_hunter_bail_enforcement`
      </Accordion>
    </AccordionGroup>
  </Accordion>

  <Accordion title="events">
    <AccordionGroup>
      <Accordion title="educational_and_training_events">
        `workshop_seminar`, `bootcamp`, `webinar`, `mastermind`,
        `virtual_summit`, `training_certification`, `hackathon`,
        `corporate_training`
      </Accordion>

      <Accordion title="conferences_and_expos">
        `conference_summit`, `convention_expo`, `industry_awards`,
        `product_launch`, `investor_demo_day`, `panel_discussion`,
        `pitch_competition`
      </Accordion>

      <Accordion title="retreats_and_wellness_events">
        `luxury_experience`, `wellness_retreat`, `womens_retreat`,
        `spiritual_retreat`, `leadership_retreat`, `digital_detox_retreat`,
        `yoga_retreat`, `plant_medicine_retreat`, `mens_retreat`,
        `couples_retreat`, `detox_retreat`, `silent_retreat`, `creative_retreat`
      </Accordion>

      <Accordion title="sports_and_fitness_events">
        `fitness_challenge`, `tournament`, `esports_tournament`,
        `outdoor_adventure`, `pickleball_tournament`, `marathon_race`,
        `fight_night`, `obstacle_course_race`, `cycling`, `swim_meet`,
        `golf_tournament`, `crossfit_competition`, `martial_arts_tournament`,
        `surfing_competition`
      </Accordion>

      <Accordion title="social_and_networking_events">
        `party`, `meetup`, `founders_dinner`, `community_gathering`, `alumni`,
        `wine_tasting`, `dinner`, `singles`, `professional_happy_hour`,
        `women_networking`, `industry_mixer`, `trivia_night`, `car_show`
      </Accordion>

      <Accordion title="performance_and_festival_events">
        `concert`, `food_festival`, `comedy_show`, `dance_performance`,
        `poetry_spoken_word`, `beer_festival`, `theater_performance`,
        `film_screening`, `music_festival`, `cultural_festival`, `fashion_show`,
        `drag_show`, `magic_show`, `art_exhibition`
      </Accordion>

      <Accordion title="community_charity_and_family_events">
        `farmers_market`, `family_festival`, `kids_and_family`, `fundraiser`,
        `awareness_campaign`, `volunteering`, `charity_auction`,
        `benefit_concert`, `charity_run_walk`, `environmental_cleanup`,
        `holiday`, `block_party`, `graduation_ceremony`, `memorial`
      </Accordion>

      <Accordion title="regulated_and_prohibited">
        `adult_content`, `hate_and_violence`, `gambling_and_sweepstakes`,
        `political_and_charitable_fundraising`
      </Accordion>
    </AccordionGroup>
  </Accordion>
</AccordionGroup>
