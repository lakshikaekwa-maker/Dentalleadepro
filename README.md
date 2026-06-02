import React, { useState, useMemo } from "react";
import {
  LayoutDashboard, Users, KanbanSquare, CalendarClock, BarChart3,
  Search, Plus, Download, Moon, Sun, Sparkles, X, Phone, Mail, Globe,
  Facebook, Instagram, Linkedin, MapPin, TrendingUp, AlertTriangle,
  ChevronRight, Filter, Star, Clock, FileText, Activity, Copy, Check,
  ArrowUpRight, Building2, Stethoscope, Trash2, Edit3, MessageSquare,
  Radar, Layers, CheckCircle2, Loader2, PlusCircle, Wand2, Info
} from "lucide-react";

// ============================= SAMPLE DATA =============================
const SEED_LEADS = [{"id":1,"practiceName":"Pine Dental Care","contactName":"Sarah Thomas","jobTitle":"Marketing Director","website":"https://www.pinedentalcare.com","facebook":"","instagram":"https://instagram.com/pinedentalcare","linkedin":"","email":"sarah@pinedentalcare.com","phone":"(423) 438-9279","state":"CA","city":"Fresno","locations":2,"dentists":1,"specialty":"Periodontics","source":"Website","score":48,"status":"New","lastContact":"","nextFollowUp":"2026-06-17","notes":"Budget approved, finalizing scope."},{"id":2,"practiceName":"Maple Dental Associates","contactName":"James Hernandez","jobTitle":"Practice Manager","website":"https://www.mapledentalassociates.com","facebook":"https://facebook.com/mapledentalassociates","instagram":"https://instagram.com/mapledentalassociates","linkedin":"","email":"james@mapledentalassociates.com","phone":"(299) 567-6635","state":"MI","city":"Grand Rapids","locations":1,"dentists":3,"specialty":"General Dentistry","source":"Website","score":78,"status":"Proposal Sent","lastContact":"2026-05-03","nextFollowUp":"2026-06-14","notes":"Asked for a callback next week. Busy season."},{"id":3,"practiceName":"Heritage Orthodontics","contactName":"Charles White","jobTitle":"Practice Manager","website":"https://www.heritageorthodontics.com","facebook":"https://facebook.com/heritageorthodontics","instagram":"https://instagram.com/heritageorthodontics","linkedin":"https://linkedin.com/company/heritageorthodontics","email":"charles@heritageorthodontics.com","phone":"(303) 589-5554","state":"TX","city":"Fort Worth","locations":1,"dentists":4,"specialty":"Oral Surgery","source":"Instagram","score":43,"status":"Meeting Scheduled","lastContact":"2026-05-09","nextFollowUp":"2026-06-08","notes":"Very responsive, wants a demo of the platform."},{"id":4,"practiceName":"Meadow Smiles","contactName":"Dr. Joshua Hernandez","jobTitle":"Lead Dentist","website":"https://www.meadowsmiles.com","facebook":"https://facebook.com/meadowsmiles","instagram":"https://instagram.com/meadowsmiles","linkedin":"https://linkedin.com/company/meadowsmiles","email":"joshua@meadowsmiles.com","phone":"(424) 901-6313","state":"WA","city":"Seattle","locations":1,"dentists":1,"specialty":"Periodontics","source":"Other","score":66,"status":"Meeting Scheduled","lastContact":"2026-05-30","nextFollowUp":"2026-06-22","notes":"Needs buy-in from partner dentist before proceeding."},{"id":5,"practiceName":"Gentle Smiles","contactName":"Dr. Andrew Lee","jobTitle":"DDS","website":"https://www.gentlesmiles.com","facebook":"https://facebook.com/gentlesmiles","instagram":"","linkedin":"","email":"andrew@gentlesmiles.com","phone":"(774) 751-5304","state":"TX","city":"Dallas","locations":1,"dentists":4,"specialty":"Prosthodontics","source":"LinkedIn","score":98,"status":"New","lastContact":"","nextFollowUp":"2026-06-04","notes":"Currently using a competitor, contract ends Q3."},{"id":6,"practiceName":"Aspen Dental Associates","contactName":"Dr. Linda Martinez","jobTitle":"Lead Dentist","website":"https://www.aspendentalassociates.com","facebook":"https://facebook.com/aspendentalassociates","instagram":"https://instagram.com/aspendentalassociates","linkedin":"https://linkedin.com/company/aspendentalassociates","email":"linda@aspendentalassociates.com","phone":"(766) 211-2876","state":"TX","city":"Houston","locations":2,"dentists":5,"specialty":"Oral Surgery","source":"Facebook","score":89,"status":"Proposal Sent","lastContact":"2026-05-14","nextFollowUp":"2026-06-10","notes":"Currently using a competitor, contract ends Q3."},{"id":7,"practiceName":"Modern Dental","contactName":"Steven Lopez","jobTitle":"Office Manager","website":"https://www.moderndental.com","facebook":"https://facebook.com/moderndental","instagram":"https://instagram.com/moderndental","linkedin":"","email":"steven@moderndental.com","phone":"(582) 980-3646","state":"WA","city":"Spokane","locations":4,"dentists":1,"specialty":"Oral Surgery","source":"Google Search","score":48,"status":"Meeting Scheduled","lastContact":"2026-05-31","nextFollowUp":"2026-05-31","notes":"Needs buy-in from partner dentist before proceeding."},{"id":8,"practiceName":"Grand Smiles","contactName":"Jessica Reed","jobTitle":"Operations Manager","website":"https://www.grandsmiles.com","facebook":"","instagram":"https://instagram.com/grandsmiles","linkedin":"https://linkedin.com/company/grandsmiles","email":"jessica@grandsmiles.com","phone":"(331) 875-8787","state":"NY","city":"New York","locations":3,"dentists":9,"specialty":"Pediatric Dentistry","source":"LinkedIn","score":45,"status":"Lost","lastContact":"2026-04-29","nextFollowUp":"","notes":"Budget approved, finalizing scope."},{"id":9,"practiceName":"Cedar Dental Center","contactName":"Raj Martin","jobTitle":"Marketing Director","website":"https://www.cedardentalcenter.com","facebook":"https://facebook.com/cedardentalcenter","instagram":"","linkedin":"","email":"raj@cedardentalcenter.com","phone":"(221) 802-4770","state":"CO","city":"Denver","locations":1,"dentists":2,"specialty":"General Dentistry","source":"Facebook","score":82,"status":"Contacted","lastContact":"2026-04-17","nextFollowUp":"2026-06-17","notes":"Interested in growth marketing for new location."},{"id":10,"practiceName":"Maple Dental Care","contactName":"Nancy Jones","jobTitle":"Practice Manager","website":"https://www.mapledentalcare.com","facebook":"https://facebook.com/mapledentalcare","instagram":"https://instagram.com/mapledentalcare","linkedin":"https://linkedin.com/company/mapledentalcare","email":"nancy@mapledentalcare.com","phone":"(790) 684-4981","state":"TN","city":"Nashville","locations":2,"dentists":8,"specialty":"Prosthodontics","source":"Instagram","score":70,"status":"Meeting Scheduled","lastContact":"2026-05-26","nextFollowUp":"2026-05-31","notes":"Budget approved, finalizing scope."},{"id":11,"practiceName":"Premier Orthodontics","contactName":"Dr. Maria Brown","jobTitle":"DDS","website":"https://www.premierorthodontics.com","facebook":"https://facebook.com/premierorthodontics","instagram":"https://instagram.com/premierorthodontics","linkedin":"","email":"maria@premierorthodontics.com","phone":"(394) 749-8350","state":"OH","city":"Cleveland","locations":1,"dentists":2,"specialty":"Prosthodontics","source":"Instagram","score":47,"status":"Proposal Sent","lastContact":"2026-05-15","nextFollowUp":"2026-06-11","notes":"Very responsive, wants a demo of the platform."},{"id":12,"practiceName":"Metro Dental Care","contactName":"Patricia Murphy","jobTitle":"Operations Manager","website":"https://www.metrodentalcare.com","facebook":"https://facebook.com/metrodentalcare","instagram":"https://instagram.com/metrodentalcare","linkedin":"","email":"patricia@metrodentalcare.com","phone":"(610) 260-3697","state":"GA","city":"Atlanta","locations":5,"dentists":13,"specialty":"General Dentistry","source":"Google Search","score":46,"status":"Won","lastContact":"2026-05-16","nextFollowUp":"","notes":"Cold outreach, no response yet."},{"id":13,"practiceName":"Coastal Orthodontics","contactName":"Elizabeth Gonzalez","jobTitle":"Marketing Director","website":"https://www.coastalorthodontics.com","facebook":"","instagram":"https://instagram.com/coastalorthodontics","linkedin":"","email":"elizabeth@coastalorthodontics.com","phone":"(688) 714-9701","state":"WA","city":"Spokane","locations":2,"dentists":3,"specialty":"General Dentistry","source":"Referral","score":42,"status":"Contacted","lastContact":"2026-05-27","nextFollowUp":"2026-06-02","notes":"Asked for a callback next week. Busy season."},{"id":14,"practiceName":"Oak Dental Care","contactName":"Dr. Betty Davis","jobTitle":"Owner / Dentist","website":"https://www.oakdentalcare.com","facebook":"https://facebook.com/oakdentalcare","instagram":"https://instagram.com/oakdentalcare","linkedin":"https://linkedin.com/company/oakdentalcare","email":"betty@oakdentalcare.com","phone":"(735) 523-5272","state":"AZ","city":"Phoenix","locations":2,"dentists":4,"specialty":"Oral Surgery","source":"Instagram","score":66,"status":"Lost","lastContact":"2026-05-16","nextFollowUp":"","notes":"Budget approved, finalizing scope."},{"id":15,"practiceName":"River Smiles","contactName":"John Smith","jobTitle":"Practice Manager","website":"https://www.riversmiles.com","facebook":"https://facebook.com/riversmiles","instagram":"","linkedin":"","email":"john@riversmiles.com","phone":"(270) 450-7054","state":"GA","city":"Savannah","locations":1,"dentists":3,"specialty":"Pediatric Dentistry","source":"Google Search","score":47,"status":"Contacted","lastContact":"2026-04-09","nextFollowUp":"2026-06-14","notes":"Referred by existing client. Warm lead."},{"id":16,"practiceName":"Oak Dental Center","contactName":"Donna Martin","jobTitle":"Practice Manager","website":"https://www.oakdentalcenter.com","facebook":"https://facebook.com/oakdentalcenter","instagram":"https://instagram.com/oakdentalcenter","linkedin":"https://linkedin.com/company/oakdentalcenter","email":"donna@oakdentalcenter.com","phone":"(359) 478-5616","state":"CA","city":"Fresno","locations":5,"dentists":20,"specialty":"Periodontics","source":"Website","score":48,"status":"Lost","lastContact":"2026-05-11","nextFollowUp":"","notes":"Very responsive, wants a demo of the platform."},{"id":17,"practiceName":"Sunrise Dental","contactName":"Sarah Williams","jobTitle":"Office Manager","website":"https://www.sunrisedental.com","facebook":"","instagram":"https://instagram.com/sunrisedental","linkedin":"https://linkedin.com/company/sunrisedental","email":"sarah@sunrisedental.com","phone":"(774) 209-2832","state":"TX","city":"San Antonio","locations":2,"dentists":2,"specialty":"Pediatric Dentistry","source":"Referral","score":51,"status":"New","lastContact":"","nextFollowUp":"2026-05-29","notes":"Needs buy-in from partner dentist before proceeding."},{"id":18,"practiceName":"Harbor Dental Center","contactName":"David Williams","jobTitle":"Practice Manager","website":"https://www.harbordentalcenter.com","facebook":"https://facebook.com/harbordentalcenter","instagram":"https://instagram.com/harbordentalcenter","linkedin":"","email":"david@harbordentalcenter.com","phone":"(773) 616-3532","state":"FL","city":"Jacksonville","locations":4,"dentists":8,"specialty":"Pediatric Dentistry","source":"Other","score":40,"status":"Contacted","lastContact":"2026-04-11","nextFollowUp":"2026-06-02","notes":"Budget approved, finalizing scope."},{"id":19,"practiceName":"Bright Family Dentistry","contactName":"Dr. Anthony Thomas","jobTitle":"Owner / Dentist","website":"https://www.brightfamilydentistry.com","facebook":"https://facebook.com/brightfamilydentistry","instagram":"","linkedin":"https://linkedin.com/company/brightfamilydentistry","email":"anthony@brightfamilydentistry.com","phone":"(671) 558-6000","state":"WA","city":"Spokane","locations":1,"dentists":2,"specialty":"Periodontics","source":"Facebook","score":48,"status":"New","lastContact":"","nextFollowUp":"2026-06-18","notes":"Very responsive, wants a demo of the platform."},{"id":20,"practiceName":"Gentle Dental Studio","contactName":"Sarah Thompson","jobTitle":"Operations Manager","website":"https://www.gentledentalstudio.com","facebook":"https://facebook.com/gentledentalstudio","instagram":"https://instagram.com/gentledentalstudio","linkedin":"","email":"sarah@gentledentalstudio.com","phone":"(467) 382-5349","state":"IL","city":"Chicago","locations":2,"dentists":1,"specialty":"Orthodontics","source":"Referral","score":86,"status":"Proposal Sent","lastContact":"2026-05-05","nextFollowUp":"2026-06-08","notes":"Needs buy-in from partner dentist before proceeding."},{"id":21,"practiceName":"Family Cosmetic Dentistry","contactName":"Dr. Matthew Reed","jobTitle":"Owner / Dentist","website":"https://www.familycosmeticdentistry.com","facebook":"https://facebook.com/familycosmeticdentistry","instagram":"https://instagram.com/familycosmeticdentistry","linkedin":"https://linkedin.com/company/familycosmeticdentistry","email":"matthew@familycosmeticdentistry.com","phone":"(959) 954-4228","state":"NC","city":"Charlotte","locations":3,"dentists":6,"specialty":"Prosthodontics","source":"Facebook","score":40,"status":"New","lastContact":"","nextFollowUp":"2026-06-18","notes":"Needs buy-in from partner dentist before proceeding."},{"id":22,"practiceName":"Oak Dental Studio","contactName":"Dr. Maria Martin","jobTitle":"DDS","website":"https://www.oakdentalstudio.com","facebook":"https://facebook.com/oakdentalstudio","instagram":"","linkedin":"","email":"maria@oakdentalstudio.com","phone":"(588) 893-3851","state":"AZ","city":"Phoenix","locations":1,"dentists":3,"specialty":"Prosthodontics","source":"Referral","score":87,"status":"Meeting Scheduled","lastContact":"2026-04-09","nextFollowUp":"2026-05-28","notes":"Referred by existing client. Warm lead."},{"id":23,"practiceName":"Coastal Dental Group","contactName":"Joshua Lee","jobTitle":"Marketing Director","website":"https://www.coastaldentalgroup.com","facebook":"https://facebook.com/coastaldentalgroup","instagram":"https://instagram.com/coastaldentalgroup","linkedin":"https://linkedin.com/company/coastaldentalgroup","email":"joshua@coastaldentalgroup.com","phone":"(373) 874-2389","state":"OH","city":"Cincinnati","locations":2,"dentists":5,"specialty":"Oral Surgery","source":"Facebook","score":91,"status":"Contacted","lastContact":"2026-04-10","nextFollowUp":"2026-06-21","notes":"Very responsive, wants a demo of the platform."},{"id":24,"practiceName":"Maple Dental Group","contactName":"Dr. Robert Thomas","jobTitle":"DDS","website":"https://www.mapledentalgroup.com","facebook":"https://facebook.com/mapledentalgroup","instagram":"https://instagram.com/mapledentalgroup","linkedin":"","email":"robert@mapledentalgroup.com","phone":"(609) 449-3417","state":"FL","city":"Miami","locations":2,"dentists":1,"specialty":"Orthodontics","source":"Other","score":44,"status":"Lost","lastContact":"2026-05-05","nextFollowUp":"","notes":"Very responsive, wants a demo of the platform."},{"id":25,"practiceName":"Park Dental Center","contactName":"Donna Thomas","jobTitle":"Operations Manager","website":"https://www.parkdentalcenter.com","facebook":"https://facebook.com/parkdentalcenter","instagram":"https://instagram.com/parkdentalcenter","linkedin":"https://linkedin.com/company/parkdentalcenter","email":"donna@parkdentalcenter.com","phone":"(524) 973-8251","state":"GA","city":"Atlanta","locations":2,"dentists":7,"specialty":"Cosmetic Dentistry","source":"Instagram","score":50,"status":"Lost","lastContact":"2026-04-15","nextFollowUp":"","notes":"Cold outreach, no response yet."},{"id":26,"practiceName":"Modern Smiles","contactName":"Amanda Moore","jobTitle":"Office Manager","website":"https://www.modernsmiles.com","facebook":"https://facebook.com/modernsmiles","instagram":"https://instagram.com/modernsmiles","linkedin":"","email":"amanda@modernsmiles.com","phone":"(543) 527-9849","state":"CO","city":"Denver","locations":2,"dentists":2,"specialty":"Pediatric Dentistry","source":"Instagram","score":97,"status":"Negotiation","lastContact":"2026-05-18","nextFollowUp":"2026-06-09","notes":"Currently using a competitor, contract ends Q3."},{"id":27,"practiceName":"Crystal Dental Group","contactName":"Anthony Perez","jobTitle":"Operations Manager","website":"https://www.crystaldentalgroup.com","facebook":"https://facebook.com/crystaldentalgroup","instagram":"https://instagram.com/crystaldentalgroup","linkedin":"https://linkedin.com/company/crystaldentalgroup","email":"anthony@crystaldentalgroup.com","phone":"(912) 220-7232","state":"TX","city":"San Antonio","locations":1,"dentists":4,"specialty":"General Dentistry","source":"LinkedIn","score":88,"status":"Meeting Scheduled","lastContact":"2026-05-13","nextFollowUp":"2026-06-21","notes":"Budget approved, finalizing scope."},{"id":28,"practiceName":"Metro Orthodontics","contactName":"Kimberly Anderson","jobTitle":"Office Manager","website":"https://www.metroorthodontics.com","facebook":"https://facebook.com/metroorthodontics","instagram":"https://instagram.com/metroorthodontics","linkedin":"https://linkedin.com/company/metroorthodontics","email":"kimberly@metroorthodontics.com","phone":"(678) 330-9751","state":"NC","city":"Charlotte","locations":1,"dentists":1,"specialty":"Prosthodontics","source":"Referral","score":97,"status":"New","lastContact":"","nextFollowUp":"2026-06-15","notes":"Interested in growth marketing for new location."},{"id":29,"practiceName":"Summit Orthodontics","contactName":"Barbara Brown","jobTitle":"Office Manager","website":"https://www.summitorthodontics.com","facebook":"https://facebook.com/summitorthodontics","instagram":"https://instagram.com/summitorthodontics","linkedin":"","email":"barbara@summitorthodontics.com","phone":"(631) 458-2341","state":"FL","city":"Jacksonville","locations":1,"dentists":4,"specialty":"General Dentistry","source":"Website","score":76,"status":"New","lastContact":"","nextFollowUp":"2026-06-14","notes":"Interested in growth marketing for new location."},{"id":30,"practiceName":"Premier Dental Group","contactName":"Dr. Priya Williams","jobTitle":"Owner / Dentist","website":"https://www.premierdentalgroup.com","facebook":"https://facebook.com/premierdentalgroup","instagram":"","linkedin":"","email":"priya@premierdentalgroup.com","phone":"(317) 777-4571","state":"AZ","city":"Phoenix","locations":1,"dentists":4,"specialty":"Endodontics","source":"Other","score":66,"status":"Negotiation","lastContact":"2026-05-09","nextFollowUp":"2026-06-02","notes":"Asked for a callback next week. Busy season."},{"id":31,"practiceName":"Blue Sky Family Dentistry","contactName":"Dr. Michelle Johnson","jobTitle":"DDS","website":"https://www.blueskyfamilydentistry.com","facebook":"","instagram":"https://instagram.com/blueskyfamilydentistry","linkedin":"https://linkedin.com/company/blueskyfamilydentistry","email":"michelle@blueskyfamilydentistry.com","phone":"(448) 304-5941","state":"IL","city":"Chicago","locations":1,"dentists":1,"specialty":"General Dentistry","source":"LinkedIn","score":83,"status":"Lost","lastContact":"2026-04-28","nextFollowUp":"","notes":"Budget approved, finalizing scope."},{"id":32,"practiceName":"Meadow Dental Studio","contactName":"Dr. Priya Perez","jobTitle":"Owner / Dentist","website":"https://www.meadowdentalstudio.com","facebook":"https://facebook.com/meadowdentalstudio","instagram":"https://instagram.com/meadowdentalstudio","linkedin":"https://linkedin.com/company/meadowdentalstudio","email":"priya@meadowdentalstudio.com","phone":"(670) 924-3506","state":"TX","city":"Fort Worth","locations":4,"dentists":14,"specialty":"Pediatric Dentistry","source":"Website","score":88,"status":"New","lastContact":"","nextFollowUp":"2026-06-13","notes":"Referred by existing client. Warm lead."},{"id":33,"practiceName":"Blue Sky Dental Associates","contactName":"Dr. Maria Bailey","jobTitle":"Owner / Dentist","website":"https://www.blueskydentalassociates.com","facebook":"https://facebook.com/blueskydentalassociates","instagram":"https://instagram.com/blueskydentalassociates","linkedin":"https://linkedin.com/company/blueskydentalassociates","email":"maria@blueskydentalassociates.com","phone":"(783) 824-7209","state":"GA","city":"Savannah","locations":4,"dentists":11,"specialty":"General Dentistry","source":"Google Search","score":66,"status":"New","lastContact":"","nextFollowUp":"2026-06-07","notes":"Currently using a competitor, contract ends Q3."},{"id":34,"practiceName":"Smile Dental Group","contactName":"Nancy Moore","jobTitle":"Marketing Director","website":"https://www.smiledentalgroup.com","facebook":"https://facebook.com/smiledentalgroup","instagram":"","linkedin":"https://linkedin.com/company/smiledentalgroup","email":"nancy@smiledentalgroup.com","phone":"(700) 768-4937","state":"PA","city":"Pittsburgh","locations":2,"dentists":8,"specialty":"Cosmetic Dentistry","source":"Google Search","score":70,"status":"Won","lastContact":"2026-04-12","nextFollowUp":"","notes":"Interested in growth marketing for new location."},{"id":35,"practiceName":"Summit Smiles","contactName":"Aisha Thomas","jobTitle":"Marketing Director","website":"https://www.summitsmiles.com","facebook":"https://facebook.com/summitsmiles","instagram":"https://instagram.com/summitsmiles","linkedin":"https://linkedin.com/company/summitsmiles","email":"aisha@summitsmiles.com","phone":"(560) 919-8434","state":"NY","city":"Albany","locations":2,"dentists":5,"specialty":"Endodontics","source":"LinkedIn","score":82,"status":"Contacted","lastContact":"2026-05-18","nextFollowUp":"2026-05-31","notes":"Very responsive, wants a demo of the platform."},{"id":36,"practiceName":"Valley Dental Care","contactName":"Richard Wilson","jobTitle":"Operations Manager","website":"https://www.valleydentalcare.com","facebook":"https://facebook.com/valleydentalcare","instagram":"","linkedin":"","email":"richard@valleydentalcare.com","phone":"(398) 503-4727","state":"WA","city":"Seattle","locations":3,"dentists":6,"specialty":"Pediatric Dentistry","source":"LinkedIn","score":70,"status":"Negotiation","lastContact":"2026-06-01","nextFollowUp":"2026-06-19","notes":"Currently using a competitor, contract ends Q3."},{"id":37,"practiceName":"Pearl Dental Center","contactName":"Dr. David Wong","jobTitle":"Lead Dentist","website":"https://www.pearldentalcenter.com","facebook":"https://facebook.com/pearldentalcenter","instagram":"https://instagram.com/pearldentalcenter","linkedin":"https://linkedin.com/company/pearldentalcenter","email":"david@pearldentalcenter.com","phone":"(458) 689-2869","state":"IL","city":"Naperville","locations":2,"dentists":2,"specialty":"Prosthodontics","source":"Google Search","score":36,"status":"New","lastContact":"","nextFollowUp":"2026-05-30","notes":"Interested in growth marketing for new location."},{"id":38,"practiceName":"River Family Dentistry","contactName":"Jennifer Thomas","jobTitle":"Operations Manager","website":"https://www.riverfamilydentistry.com","facebook":"https://facebook.com/riverfamilydentistry","instagram":"https://instagram.com/riverfamilydentistry","linkedin":"","email":"jennifer@riverfamilydentistry.com","phone":"(653) 504-8025","state":"CO","city":"Boulder","locations":2,"dentists":5,"specialty":"General Dentistry","source":"Referral","score":88,"status":"New","lastContact":"","nextFollowUp":"2026-06-20","notes":"Asked for a callback next week. Busy season."},{"id":39,"practiceName":"Blue Sky Dental Group","contactName":"Thomas Garcia","jobTitle":"Practice Manager","website":"https://www.blueskydentalgroup.com","facebook":"","instagram":"https://instagram.com/blueskydentalgroup","linkedin":"https://linkedin.com/company/blueskydentalgroup","email":"thomas@blueskydentalgroup.com","phone":"(498) 233-4792","state":"AZ","city":"Phoenix","locations":2,"dentists":5,"specialty":"Endodontics","source":"Website","score":57,"status":"New","lastContact":"","nextFollowUp":"2026-06-11","notes":"Asked for a callback next week. Busy season."},{"id":40,"practiceName":"Meadow Dental Group","contactName":"Amanda Bailey","jobTitle":"Marketing Director","website":"https://www.meadowdentalgroup.com","facebook":"https://facebook.com/meadowdentalgroup","instagram":"","linkedin":"","email":"amanda@meadowdentalgroup.com","phone":"(345) 273-1977","state":"TN","city":"Memphis","locations":1,"dentists":2,"specialty":"Endodontics","source":"Referral","score":60,"status":"Proposal Sent","lastContact":"2026-04-15","nextFollowUp":"2026-06-23","notes":"Referred by existing client. Warm lead."},{"id":41,"practiceName":"Modern Dental Care","contactName":"Aisha Sanchez","jobTitle":"Marketing Director","website":"https://www.moderndentalcare.com","facebook":"","instagram":"https://instagram.com/moderndentalcare","linkedin":"","email":"aisha@moderndentalcare.com","phone":"(456) 226-2496","state":"GA","city":"Savannah","locations":1,"dentists":2,"specialty":"General Dentistry","source":"Other","score":98,"status":"Lost","lastContact":"2026-04-19","nextFollowUp":"","notes":"Referred by existing client. Warm lead."},{"id":42,"practiceName":"Harbor Dental","contactName":"Donald Cohen","jobTitle":"Marketing Director","website":"https://www.harbordental.com","facebook":"https://facebook.com/harbordental","instagram":"https://instagram.com/harbordental","linkedin":"https://linkedin.com/company/harbordental","email":"donald@harbordental.com","phone":"(681) 556-7690","state":"CO","city":"Denver","locations":1,"dentists":3,"specialty":"Oral Surgery","source":"Website","score":70,"status":"Proposal Sent","lastContact":"2026-05-26","nextFollowUp":"2026-06-02","notes":"Needs buy-in from partner dentist before proceeding."},{"id":43,"practiceName":"Family Dental Associates","contactName":"Betty Cooper","jobTitle":"Office Manager","website":"https://www.familydentalassociates.com","facebook":"","instagram":"https://instagram.com/familydentalassociates","linkedin":"https://linkedin.com/company/familydentalassociates","email":"betty@familydentalassociates.com","phone":"(201) 873-9889","state":"IL","city":"Naperville","locations":1,"dentists":4,"specialty":"Prosthodontics","source":"Facebook","score":46,"status":"New","lastContact":"","nextFollowUp":"2026-06-03","notes":"Needs buy-in from partner dentist before proceeding."},{"id":44,"practiceName":"Oak Dental Associates","contactName":"Dr. Patricia Wilson","jobTitle":"Lead Dentist","website":"https://www.oakdentalassociates.com","facebook":"https://facebook.com/oakdentalassociates","instagram":"https://instagram.com/oakdentalassociates","linkedin":"","email":"patricia@oakdentalassociates.com","phone":"(845) 823-4920","state":"AZ","city":"Tucson","locations":5,"dentists":6,"specialty":"Endodontics","source":"Referral","score":51,"status":"New","lastContact":"","nextFollowUp":"2026-05-28","notes":"Budget approved, finalizing scope."},{"id":45,"practiceName":"Summit Dental Group","contactName":"Dr. Ashley Davis","jobTitle":"DDS","website":"https://www.summitdentalgroup.com","facebook":"https://facebook.com/summitdentalgroup","instagram":"https://instagram.com/summitdentalgroup","linkedin":"","email":"ashley@summitdentalgroup.com","phone":"(694) 683-4993","state":"MA","city":"Boston","locations":2,"dentists":8,"specialty":"Pediatric Dentistry","source":"Google Search","score":54,"status":"Proposal Sent","lastContact":"2026-05-20","nextFollowUp":"2026-06-16","notes":"Currently using a competitor, contract ends Q3."},{"id":46,"practiceName":"Sunrise Orthodontics","contactName":"Dr. James Jackson","jobTitle":"Lead Dentist","website":"https://www.sunriseorthodontics.com","facebook":"https://facebook.com/sunriseorthodontics","instagram":"https://instagram.com/sunriseorthodontics","linkedin":"https://linkedin.com/company/sunriseorthodontics","email":"james@sunriseorthodontics.com","phone":"(756) 586-8460","state":"PA","city":"Pittsburgh","locations":4,"dentists":11,"specialty":"Periodontics","source":"Website","score":97,"status":"Negotiation","lastContact":"2026-05-17","nextFollowUp":"2026-06-15","notes":"Budget approved, finalizing scope."},{"id":47,"practiceName":"Maple Orthodontics","contactName":"Dr. Donald Harris","jobTitle":"Owner / Dentist","website":"https://www.mapleorthodontics.com","facebook":"","instagram":"https://instagram.com/mapleorthodontics","linkedin":"","email":"donald@mapleorthodontics.com","phone":"(302) 650-2633","state":"CA","city":"San Jose","locations":2,"dentists":8,"specialty":"General Dentistry","source":"Website","score":54,"status":"Contacted","lastContact":"2026-05-23","nextFollowUp":"2026-06-10","notes":"Currently using a competitor, contract ends Q3."},{"id":48,"practiceName":"Summit Dental Associates","contactName":"Nancy Sanchez","jobTitle":"Operations Manager","website":"https://www.summitdentalassociates.com","facebook":"https://facebook.com/summitdentalassociates","instagram":"https://instagram.com/summitdentalassociates","linkedin":"https://linkedin.com/company/summitdentalassociates","email":"nancy@summitdentalassociates.com","phone":"(977) 699-9864","state":"CO","city":"Boulder","locations":4,"dentists":2,"specialty":"Orthodontics","source":"Instagram","score":77,"status":"Proposal Sent","lastContact":"2026-04-22","nextFollowUp":"2026-06-18","notes":"Referred by existing client. Warm lead."},{"id":49,"practiceName":"Family Dental Care","contactName":"Mark Hernandez","jobTitle":"Operations Manager","website":"https://www.familydentalcare.com","facebook":"","instagram":"https://instagram.com/familydentalcare","linkedin":"","email":"mark@familydentalcare.com","phone":"(450) 743-7751","state":"CO","city":"Denver","locations":1,"dentists":2,"specialty":"Pediatric Dentistry","source":"Instagram","score":38,"status":"Proposal Sent","lastContact":"2026-05-27","nextFollowUp":"2026-06-16","notes":"Budget approved, finalizing scope."},{"id":50,"practiceName":"Oak Dental Group","contactName":"Dr. Joseph Kim","jobTitle":"Owner / Dentist","website":"https://www.oakdentalgroup.com","facebook":"https://facebook.com/oakdentalgroup","instagram":"https://instagram.com/oakdentalgroup","linkedin":"","email":"joseph@oakdentalgroup.com","phone":"(759) 361-2210","state":"GA","city":"Atlanta","locations":1,"dentists":4,"specialty":"Oral Surgery","source":"Referral","score":93,"status":"Proposal Sent","lastContact":"2026-05-13","nextFollowUp":"2026-06-17","notes":"Budget approved, finalizing scope."}];

const SOURCES = ["Facebook","Instagram","LinkedIn","Google Search","Referral","Website","Other"];
const STAGES = ["New","Contacted","Meeting Scheduled","Proposal Sent","Negotiation","Won","Lost"];
const SPECIALTIES = ["General Dentistry","Orthodontics","Pediatric Dentistry","Periodontics","Endodontics","Oral Surgery","Prosthodontics","Cosmetic Dentistry"];
const STAGE_COLOR = {
  "New":"#60a5fa","Contacted":"#a78bfa","Meeting Scheduled":"#22d3ee",
  "Proposal Sent":"#fbbf24","Negotiation":"#fb923c","Won":"#34d399","Lost":"#f87171"
};
const SOURCE_COLOR = {
  "Facebook":"#1877f2","Instagram":"#e1306c","LinkedIn":"#0a66c2",
  "Google Search":"#ea4335","Referral":"#10b981","Website":"#8b5cf6","Other":"#94a3b8","Discovery":"#22d3ee"
};
const TODAY = "2026-06-02";

// ============================= HELPERS =============================
const fmtDate = (d) => d ? new Date(d+"T00:00:00").toLocaleDateString("en-US",{month:"short",day:"numeric",year:"numeric"}) : "—";
const daysFromToday = (d) => d ? Math.round((new Date(d+"T00:00:00") - new Date(TODAY+"T00:00:00"))/86400000) : null;
const scoreColor = (s) => s>=80?"#34d399":s>=60?"#fbbf24":s>=40?"#fb923c":"#f87171";
const uid = () => Math.random().toString(36).slice(2,9);

// Simulated AI generation (mirrors the Anthropic API call you'd wire in production)
function aiGenerate(kind, lead) {
  const fn = (lead.contactName||"there").replace(/^Dr\.\s*/,"").split(" ")[0];
  const p = lead.practiceName;
  switch(kind){
    case "Analyze Practice": return `PRACTICE ANALYSIS — ${p}\n\n• Footprint: ${lead.locations} location(s), ${lead.dentists} dentist(s) in ${lead.city}, ${lead.state}.\n• Specialty focus: ${lead.specialty}.\n• Digital presence: ${[lead.website&&"website",lead.facebook&&"Facebook",lead.instagram&&"Instagram",lead.linkedin&&"LinkedIn"].filter(Boolean).join(", ")||"limited"}.\n• Lead score: ${lead.score}/100 — ${lead.score>=80?"high-priority, strong fit":lead.score>=60?"solid mid-tier opportunity":"nurture / long-term"}.\n• Recommended angle: Multi-location practices like this benefit most from centralized patient-acquisition. Lead with ROI on new-patient cost.`;
    case "Generate Outreach Email": return `Subject: Helping ${p} attract more high-value patients\n\nHi ${fn},\n\nI came across ${p} while researching standout ${lead.specialty.toLowerCase()} practices in ${lead.city}. Your ${lead.locations>1?`${lead.locations}-location group`:"practice"} clearly invests in patient experience.\n\nWe help dental practices like yours predictably fill the schedule with the right patients — without relying on discount marketplaces. Practices we work with typically see a measurable lift in new-patient bookings within the first 60 days.\n\nWould you be open to a quick 15-minute call this week to see if it's a fit?\n\nBest,\n[Your Name]`;
    case "Generate LinkedIn Message": return `Hi ${fn} — really impressed by what ${p} is building in ${lead.city}. I work specifically with ${lead.specialty.toLowerCase()} practices on patient acquisition and thought there might be some overlap. Open to connecting?`;
    case "Generate Facebook Message": return `Hi ${fn}! 👋 Love what ${p} is doing — your page caught my eye. I help dental practices in ${lead.state} bring in more of the patients they actually want. Mind if I send over a quick idea tailored to your practice?`;
    case "Generate Follow-Up Email": return `Subject: Following up — ${p}\n\nHi ${fn},\n\nJust circling back on my note from earlier. I know running ${lead.locations>1?"multiple locations":"a busy practice"} keeps you slammed, so I'll keep this short.\n\nIf growing new-patient volume is on your radar for this quarter, I'd love to show you exactly how we'd approach it for ${p}. Happy to work around your schedule.\n\nWorth a quick chat?\n\nBest,\n[Your Name]`;
    case "Summarize Notes": return `NOTES SUMMARY — ${p}\n\n${lead.notes||"No notes recorded."}\n\nKey takeaway: ${lead.status==="New"?"Fresh lead — prioritize first touch.":lead.status==="Won"?"Closed won — transition to onboarding.":lead.status==="Lost"?"Closed lost — revisit in 2 quarters.":"Active in pipeline — maintain momentum with timely follow-up."}\nNext step: ${lead.nextFollowUp?`Follow up by ${fmtDate(lead.nextFollowUp)}.`:"Schedule next touchpoint."}`;
    default: return "";
  }
}

function downloadCSV(rows){
  const cols = ["practiceName","contactName","jobTitle","email","phone","city","state","locations","dentists","specialty","source","score","status","lastContact","nextFollowUp","website"];
  const head = cols.join(",");
  const body = rows.map(r=>cols.map(c=>{
    const v = (r[c]??"").toString().replace(/"/g,'""');
    return /[",\n]/.test(v)?`"${v}"`:v;
  }).join(",")).join("\n");
  const blob = new Blob([head+"\n"+body],{type:"text/csv"});
  const url = URL.createObjectURL(blob);
  const a = document.createElement("a");
  a.href=url; a.download="dentallead_export.csv"; a.click();
  URL.revokeObjectURL(url);
}
function downloadExcel(rows){
  // Excel-readable HTML table (.xls) — opens natively in Excel
  const cols = ["practiceName","contactName","jobTitle","email","phone","city","state","locations","dentists","specialty","source","score","status","lastContact","nextFollowUp"];
  const th = cols.map(c=>`<th>${c}</th>`).join("");
  const trs = rows.map(r=>`<tr>${cols.map(c=>`<td>${r[c]??""}</td>`).join("")}</tr>`).join("");
  const html = `<table border="1"><thead><tr>${th}</tr></thead><tbody>${trs}</tbody></table>`;
  const blob = new Blob([html],{type:"application/vnd.ms-excel"});
  const url = URL.createObjectURL(blob);
  const a = document.createElement("a");
  a.href=url; a.download="dentallead_export.xls"; a.click();
  URL.revokeObjectURL(url);
}

// ============================= ROOT APP =============================
export default function App(){
  const [dark, setDark] = useState(true);
  const [view, setView] = useState("dashboard");
  const [leads, setLeads] = useState(SEED_LEADS);
  const [query, setQuery] = useState("");
  const [editing, setEditing] = useState(null);   // lead object or "new"
  const [detail, setDetail] = useState(null);      // lead id for detail drawer
  const [filters, setFilters] = useState({state:"",specialty:"",status:"",source:"",minLoc:"",minScore:""});

  // ---- THEME TOKENS ----
  const T = dark ? {
    bg:"#0a0e16", surface:"#111824", surface2:"#0d1420", card:"#141d2b",
    border:"#1e2a3d", text:"#e6edf6", sub:"#8a9bb3", accent:"#22d3ee",
    accent2:"#34d399", inputBg:"#0d1420", hover:"#1a2536", shadow:"0 10px 40px -12px rgba(0,0,0,.6)"
  } : {
    bg:"#eef2f7", surface:"#ffffff", surface2:"#f6f9fc", card:"#ffffff",
    border:"#e2e8f0", text:"#0f1b2d", sub:"#5b6b82", accent:"#0891b2",
    accent2:"#059669", inputBg:"#f1f5f9", hover:"#eef4fa", shadow:"0 10px 30px -12px rgba(15,40,80,.18)"
  };

  // ---- DERIVED ----
  const filtered = useMemo(()=>{
    const q = query.trim().toLowerCase();
    return leads.filter(l=>{
      if(q && !(`${l.practiceName} ${l.contactName} ${l.email} ${l.city} ${l.state}`.toLowerCase().includes(q))) return false;
      if(filters.state && l.state!==filters.state) return false;
      if(filters.specialty && l.specialty!==filters.specialty) return false;
      if(filters.status && l.status!==filters.status) return false;
      if(filters.source && l.source!==filters.source) return false;
      if(filters.minLoc && l.locations < +filters.minLoc) return false;
      if(filters.minScore && l.score < +filters.minScore) return false;
      return true;
    });
  },[leads,query,filters]);

  // duplicate detection — same email or normalized practice name
  const dupes = useMemo(()=>{
    const byKey = {};
    leads.forEach(l=>{
      const k1 = (l.email||"").toLowerCase();
      const k2 = l.practiceName.toLowerCase().replace(/[^a-z]/g,"");
      [k1,k2].filter(Boolean).forEach(k=>{(byKey[k]=byKey[k]||[]).push(l.id);});
    });
    const set = new Set();
    Object.values(byKey).forEach(ids=>{if(ids.length>1) ids.forEach(i=>set.add(i));});
    return set;
  },[leads]);

  const saveLead = (data)=>{
    if(data.id && leads.some(l=>l.id===data.id)){
      setLeads(ls=>ls.map(l=>l.id===data.id?{...l,...data, _activity:[{t:TODAY,msg:"Lead updated"},...(l._activity||[])]}:l));
    } else {
      const id = Math.max(0,...leads.map(l=>l.id))+1;
      setLeads(ls=>[{...data,id,_activity:[{t:TODAY,msg:"Lead created"}]},...ls]);
    }
    setEditing(null);
  };
  const deleteLead = (id)=>{ setLeads(ls=>ls.filter(l=>l.id!==id)); setDetail(null); };
  const moveStage = (id,stage)=> setLeads(ls=>ls.map(l=>l.id===id?{...l,status:stage,lastContact:TODAY,_activity:[{t:TODAY,msg:`Moved to ${stage}`},...(l._activity||[])]}:l));

  // Import a discovered lead into the CRM. Returns "added" | "duplicate".
  const importLead = (data)=>{
    const key = (l)=> (l.email||"").toLowerCase() || l.practiceName.toLowerCase().replace(/[^a-z]/g,"");
    if(leads.some(l=>key(l)===key(data))) return "duplicate";
    const id = Math.max(0,...leads.map(l=>l.id))+1;
    const {id:_drop,_discoveryId,outreach, ...clean} = data;
    setLeads(ls=>[{...clean,id,status:"New",lastContact:"",
      _activity:[{t:TODAY,msg:"Imported from Lead Discovery",type:"log"}]},...ls]);
    return "added";
  };

  const detailLead = detail!=null ? leads.find(l=>l.id===detail) : null;

  const NAV = [
    ["dashboard","Dashboard",LayoutDashboard],
    ["discovery","Lead Discovery",Radar],
    ["leads","Leads",Users],
    ["pipeline","Pipeline",KanbanSquare],
    ["followups","Follow-Ups",CalendarClock],
    ["reports","Reports",BarChart3],
  ];

  return (
    <div style={{
      "--bg":T.bg,"--surface":T.surface,"--surface2":T.surface2,"--card":T.card,
      "--border":T.border,"--text":T.text,"--sub":T.sub,"--accent":T.accent,
      "--accent2":T.accent2,"--inputBg":T.inputBg,"--hover":T.hover,"--shadow":T.shadow,
      background:"var(--bg)", color:"var(--text)", minHeight:"100vh",
      fontFamily:"'Plus Jakarta Sans',-apple-system,system-ui,sans-serif",
      display:"flex", transition:"background .3s"
    }}>
      <style>{`
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&family=Sora:wght@600;700;800&display=swap');
        *{box-sizing:border-box;-webkit-tap-highlight-color:transparent}
        ::-webkit-scrollbar{width:9px;height:9px}
        ::-webkit-scrollbar-thumb{background:var(--border);border-radius:8px}
        ::-webkit-scrollbar-track{background:transparent}
        .dl-card{background:var(--card);border:1px solid var(--border);border-radius:16px}
        .dl-btn{cursor:pointer;border:none;font-family:inherit;font-weight:600;transition:all .15s;display:inline-flex;align-items:center;gap:7px}
        .dl-btn:active{transform:translateY(1px) scale(.985)}
        .dl-nav{cursor:pointer;transition:all .18s}
        .dl-nav:hover{background:var(--hover)}
        .dl-row:hover{background:var(--hover)}
        .dl-input{background:var(--inputBg);border:1px solid var(--border);color:var(--text);border-radius:10px;padding:10px 12px;font-family:inherit;font-size:13.5px;width:100%;outline:none;transition:border .15s}
        .dl-input:focus{border-color:var(--accent)}
        .dl-chip{font-size:11px;font-weight:700;padding:3px 10px;border-radius:999px;white-space:nowrap}
        .dl-kpi{animation:rise .5s cubic-bezier(.2,.8,.2,1) both}
        @keyframes rise{from{opacity:0;transform:translateY(14px)}to{opacity:1;transform:none}}
        @keyframes spin{to{transform:rotate(360deg)}}
        .dl-fade{animation:fade .35s ease both}
        @keyframes fade{from{opacity:0}to{opacity:1}}
        .dl-drag{cursor:grab}
        .dl-drag:active{cursor:grabbing}
        .kanban-col.over{outline:2px dashed var(--accent);outline-offset:-4px;background:var(--hover)}
        h1,h2,h3{font-family:'Sora',sans-serif;margin:0}
        @media(max-width:860px){.dl-sidebar{position:fixed;z-index:60;height:100%;transform:translateX(-110%);transition:transform .25s}.dl-sidebar.open{transform:none}.dl-hide-sm{display:none!important}}
      `}</style>

      <Sidebar NAV={NAV} view={view} setView={setView} T={T}/>

      <main style={{flex:1,minWidth:0,display:"flex",flexDirection:"column",maxHeight:"100vh",overflow:"hidden"}}>
        <Topbar {...{query,setQuery,dark,setDark,setEditing,view,T,filtered}}/>
        <div style={{overflowY:"auto",padding:"22px 26px 60px",flex:1}} className="dl-fade" key={view}>
          {view==="dashboard" && <Dashboard leads={leads} T={T} setView={setView}/>}
          {view==="discovery" && <Discovery {...{T,importLead,existing:leads}}/>}
          {view==="leads" && <LeadsView {...{leads:filtered,allLeads:leads,T,filters,setFilters,setEditing,setDetail,dupes}}/>}
          {view==="pipeline" && <Pipeline {...{leads:filtered,T,moveStage,setDetail}}/>}
          {view==="followups" && <FollowUps {...{leads,T,setDetail}}/>}
          {view==="reports" && <Reports {...{leads,T}}/>}
        </div>
      </main>

      {editing && <LeadModal lead={editing==="new"?null:editing} onSave={saveLead} onClose={()=>setEditing(null)} T={T}/>}
      {detailLead && <DetailDrawer lead={detailLead} T={T} onClose={()=>setDetail(null)} onEdit={(l)=>{setDetail(null);setEditing(l);}} onDelete={deleteLead} isDupe={dupes.has(detailLead.id)} onUpdate={saveLead}/>}
    </div>
  );
}

// ============================= SIDEBAR + TOPBAR =============================
function Sidebar({NAV,view,setView,T}){
  return (
    <aside className="dl-sidebar dl-hide-sm" style={{width:236,background:T.surface,borderRight:`1px solid ${T.border}`,display:"flex",flexDirection:"column",padding:"22px 16px",gap:6}}>
      <div style={{display:"flex",alignItems:"center",gap:11,padding:"4px 8px 22px"}}>
        <div style={{width:38,height:38,borderRadius:11,background:`linear-gradient(135deg,${T.accent},${T.accent2})`,display:"grid",placeItems:"center",boxShadow:`0 6px 18px -6px ${T.accent}`}}>
          <Stethoscope size={20} color="#04121a"/>
        </div>
        <div>
          <div style={{fontFamily:"Sora",fontWeight:800,fontSize:16,letterSpacing:-.3}}>DentalLead</div>
          <div style={{fontSize:10.5,color:T.accent,fontWeight:700,letterSpacing:1,marginTop:-2}}>PRO · CRM</div>
        </div>
      </div>
      {NAV.map(([k,label,Icon])=>(
        <div key={k} className="dl-nav" onClick={()=>setView(k)}
          style={{display:"flex",alignItems:"center",gap:12,padding:"11px 13px",borderRadius:11,fontWeight:600,fontSize:14,
            background:view===k?`linear-gradient(90deg,${T.accent}22,transparent)`:"transparent",
            color:view===k?T.text:T.sub,
            borderLeft:view===k?`3px solid ${T.accent}`:"3px solid transparent"}}>
          <Icon size={18} color={view===k?T.accent:T.sub}/>{label}
        </div>
      ))}
      <div style={{marginTop:"auto",padding:"14px 12px",borderRadius:13,background:`linear-gradient(135deg,${T.accent}14,${T.accent2}10)`,border:`1px solid ${T.border}`}}>
        <div style={{display:"flex",alignItems:"center",gap:8,fontWeight:700,fontSize:13}}><Sparkles size={15} color={T.accent}/>AI Assistant</div>
        <div style={{fontSize:11.5,color:T.sub,marginTop:6,lineHeight:1.5}}>Open any lead to draft outreach, analyze practices & summarize notes.</div>
      </div>
    </aside>
  );
}

function Topbar({query,setQuery,dark,setDark,setEditing,view,T,filtered}){
  const titles={dashboard:"Dashboard",leads:"Lead Management",pipeline:"Sales Pipeline",followups:"Follow-Up Management",reports:"Reporting"};
  return (
    <header style={{display:"flex",alignItems:"center",gap:14,padding:"15px 26px",borderBottom:`1px solid ${T.border}`,background:T.surface,flexWrap:"wrap"}}>
      <h1 style={{fontSize:19,fontWeight:800,letterSpacing:-.4}}>{titles[view]}</h1>
      <div style={{position:"relative",flex:1,maxWidth:420,minWidth:160,marginLeft:8}}>
        <Search size={16} color={T.sub} style={{position:"absolute",left:12,top:11}}/>
        <input className="dl-input" style={{paddingLeft:36}} placeholder="Search practices, contacts, cities…" value={query} onChange={e=>setQuery(e.target.value)}/>
      </div>
      <button className="dl-btn" onClick={()=>setDark(d=>!d)} style={{background:T.inputBg,color:T.text,padding:"9px 11px",borderRadius:10,border:`1px solid ${T.border}`}}>
        {dark?<Sun size={16}/>:<Moon size={16}/>}
      </button>
      <button className="dl-btn dl-hide-sm" onClick={()=>downloadCSV(filtered)} style={{background:T.inputBg,color:T.text,padding:"9px 13px",borderRadius:10,border:`1px solid ${T.border}`,fontSize:13}}>
        <Download size={15}/>CSV
      </button>
      <button className="dl-btn" onClick={()=>setEditing("new")} style={{background:`linear-gradient(135deg,${T.accent},${T.accent2})`,color:"#04121a",padding:"10px 16px",borderRadius:10,fontSize:13.5,boxShadow:`0 8px 20px -8px ${T.accent}`}}>
        <Plus size={16}/>Add Lead
      </button>
    </header>
  );
}

// ============================= DASHBOARD =============================
function Dashboard({leads,T,setView}){
  const by = (s)=>leads.filter(l=>l.status===s).length;
  const total=leads.length;
  const won=by("Won"), lost=by("Lost");
  const newL=by("New"), contacted=by("Contacted");
  const followUp = leads.filter(l=>l.nextFollowUp).length;
  const qualified = leads.filter(l=>["Meeting Scheduled","Proposal Sent","Negotiation"].includes(l.status)).length;
  const convRate = total? Math.round(won/total*100):0;

  const byState = useMemo(()=>{
    const m={}; leads.forEach(l=>m[l.state]=(m[l.state]||0)+1);
    return Object.entries(m).sort((a,b)=>b[1]-a[1]).slice(0,8);
  },[leads]);
  const bySource = useMemo(()=>{
    const m={}; leads.forEach(l=>m[l.source]=(m[l.source]||0)+1);
    return SOURCES.map(s=>[s,m[s]||0]);
  },[leads]);
  const maxState = Math.max(1,...byState.map(s=>s[1]));
  const maxSrc = Math.max(1,...bySource.map(s=>s[1]));

  const KPIS = [
    ["Total Leads",total,Users,T.accent],
    ["New",newL,Sparkles,"#60a5fa"],
    ["Contacted",contacted,Phone,"#a78bfa"],
    ["Follow-Up",followUp,CalendarClock,"#fbbf24"],
    ["Qualified",qualified,Star,"#22d3ee"],
    ["Closed Won",won,TrendingUp,"#34d399"],
    ["Closed Lost",lost,X,"#f87171"],
    ["Conversion",convRate+"%",Activity,T.accent2],
  ];

  return (
    <div>
      <div style={{display:"grid",gridTemplateColumns:"repeat(auto-fit,minmax(168px,1fr))",gap:14,marginBottom:20}}>
        {KPIS.map(([label,val,Icon,c],i)=>(
          <div key={label} className="dl-card dl-kpi" style={{padding:"16px 17px",animationDelay:`${i*.05}s`,position:"relative",overflow:"hidden"}}>
            <div style={{position:"absolute",right:-10,top:-10,width:60,height:60,borderRadius:"50%",background:c,opacity:.08}}/>
            <div style={{display:"flex",justifyContent:"space-between",alignItems:"flex-start"}}>
              <div style={{fontSize:12.5,color:T.sub,fontWeight:600}}>{label}</div>
              <div style={{width:30,height:30,borderRadius:9,background:c+"22",display:"grid",placeItems:"center"}}><Icon size={16} color={c}/></div>
            </div>
            <div style={{fontFamily:"Sora",fontSize:30,fontWeight:800,marginTop:8,letterSpacing:-1}}>{val}</div>
          </div>
        ))}
      </div>

      <div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:16,marginBottom:16}}>
        <div className="dl-card" style={{padding:"18px 20px"}}>
          <div style={{display:"flex",alignItems:"center",justifyContent:"space-between",marginBottom:16}}>
            <h3 style={{fontSize:15,fontWeight:700,display:"flex",gap:8,alignItems:"center"}}><MapPin size={16} color={T.accent}/>Leads by State</h3>
            <span className="dl-chip" style={{background:T.inputBg,color:T.sub}}>Top {byState.length}</span>
          </div>
          {byState.map(([st,n])=>(
            <div key={st} style={{display:"flex",alignItems:"center",gap:12,marginBottom:11}}>
              <div style={{width:32,fontWeight:700,fontSize:13}}>{st}</div>
              <div style={{flex:1,height:9,background:T.inputBg,borderRadius:6,overflow:"hidden"}}>
                <div style={{width:`${n/maxState*100}%`,height:"100%",background:`linear-gradient(90deg,${T.accent},${T.accent2})`,borderRadius:6,transition:"width .6s"}}/>
              </div>
              <div style={{width:24,textAlign:"right",fontSize:13,fontWeight:700,color:T.sub}}>{n}</div>
            </div>
          ))}
        </div>

        <div className="dl-card" style={{padding:"18px 20px"}}>
          <h3 style={{fontSize:15,fontWeight:700,marginBottom:16,display:"flex",gap:8,alignItems:"center"}}><Globe size={16} color={T.accent}/>Leads by Source</h3>
          {bySource.map(([s,n])=>(
            <div key={s} style={{display:"flex",alignItems:"center",gap:12,marginBottom:11}}>
              <div style={{width:96,fontSize:12.5,fontWeight:600,color:T.sub}}>{s}</div>
              <div style={{flex:1,height:9,background:T.inputBg,borderRadius:6,overflow:"hidden"}}>
                <div style={{width:`${n/maxSrc*100}%`,height:"100%",background:SOURCE_COLOR[s],borderRadius:6,transition:"width .6s"}}/>
              </div>
              <div style={{width:24,textAlign:"right",fontSize:13,fontWeight:700}}>{n}</div>
            </div>
          ))}
        </div>
      </div>

      <div className="dl-card" style={{padding:"18px 20px"}}>
        <div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:14}}>
          <h3 style={{fontSize:15,fontWeight:700}}>Pipeline Distribution</h3>
          <button className="dl-btn" onClick={()=>setView("pipeline")} style={{background:"transparent",color:T.accent,fontSize:13}}>Open board <ChevronRight size={15}/></button>
        </div>
        <div style={{display:"flex",gap:6,height:40,borderRadius:10,overflow:"hidden"}}>
          {STAGES.map(s=>{const n=by(s);if(!n)return null;return(
            <div key={s} title={`${s}: ${n}`} style={{flex:n,background:STAGE_COLOR[s],display:"grid",placeItems:"center",minWidth:34}}>
              <span style={{fontSize:12,fontWeight:800,color:"#04121a"}}>{n}</span>
            </div>
          );})}
        </div>
        <div style={{display:"flex",flexWrap:"wrap",gap:14,marginTop:14}}>
          {STAGES.map(s=>(
            <div key={s} style={{display:"flex",alignItems:"center",gap:6,fontSize:12,color:T.sub}}>
              <span style={{width:10,height:10,borderRadius:3,background:STAGE_COLOR[s]}}/>{s} <b style={{color:T.text}}>{by(s)}</b>
            </div>
          ))}
        </div>
      </div>
    </div>
  );
}

// ============================= LEADS VIEW (table + filters) =============================
function LeadsView({leads,allLeads,T,filters,setFilters,setEditing,setDetail,dupes}){
  const [showF,setShowF]=useState(false);
  const set=(k,v)=>setFilters(f=>({...f,[k]:v}));
  const activeF = Object.values(filters).filter(Boolean).length;
  return (
    <div>
      <div style={{display:"flex",alignItems:"center",gap:10,marginBottom:14,flexWrap:"wrap"}}>
        <button className="dl-btn" onClick={()=>setShowF(s=>!s)} style={{background:showF?T.accent+"22":T.inputBg,color:showF?T.accent:T.text,padding:"9px 14px",borderRadius:10,border:`1px solid ${T.border}`,fontSize:13}}>
          <Filter size={15}/>Filters {activeF>0 && <span className="dl-chip" style={{background:T.accent,color:"#04121a"}}>{activeF}</span>}
        </button>
        <div style={{fontSize:13,color:T.sub}}>{leads.length} of {allLeads.length} leads</div>
        {dupes.size>0 && <span className="dl-chip" style={{background:"#f87171"+"22",color:"#f87171",display:"flex",alignItems:"center",gap:5}}><AlertTriangle size={12}/>{dupes.size} possible duplicates</span>}
        <div style={{marginLeft:"auto",display:"flex",gap:8}}>
          <button className="dl-btn" onClick={()=>downloadCSV(leads)} style={{background:T.inputBg,color:T.text,padding:"9px 13px",borderRadius:10,border:`1px solid ${T.border}`,fontSize:13}}><Download size={15}/>CSV</button>
          <button className="dl-btn" onClick={()=>downloadExcel(leads)} style={{background:T.inputBg,color:T.text,padding:"9px 13px",borderRadius:10,border:`1px solid ${T.border}`,fontSize:13}}><Download size={15}/>Excel</button>
        </div>
      </div>

      {showF && (
        <div className="dl-card dl-fade" style={{padding:"16px 18px",marginBottom:14,display:"grid",gridTemplateColumns:"repeat(auto-fit,minmax(150px,1fr))",gap:12}}>
          <Sel label="State" v={filters.state} set={v=>set("state",v)} opts={[...new Set(allLeads.map(l=>l.state))].sort()} T={T}/>
          <Sel label="Specialty" v={filters.specialty} set={v=>set("specialty",v)} opts={SPECIALTIES} T={T}/>
          <Sel label="Status" v={filters.status} set={v=>set("status",v)} opts={STAGES} T={T}/>
          <Sel label="Source" v={filters.source} set={v=>set("source",v)} opts={SOURCES} T={T}/>
          <div><label style={{fontSize:11.5,color:T.sub,fontWeight:600}}>Min Locations</label><input className="dl-input" type="number" value={filters.minLoc} onChange={e=>set("minLoc",e.target.value)} placeholder="any"/></div>
          <div><label style={{fontSize:11.5,color:T.sub,fontWeight:600}}>Min Score</label><input className="dl-input" type="number" value={filters.minScore} onChange={e=>set("minScore",e.target.value)} placeholder="any"/></div>
          <div style={{display:"flex",alignItems:"flex-end"}}><button className="dl-btn" onClick={()=>setFilters({state:"",specialty:"",status:"",source:"",minLoc:"",minScore:""})} style={{background:T.inputBg,color:T.sub,padding:"10px 14px",borderRadius:10,border:`1px solid ${T.border}`,fontSize:13}}>Clear all</button></div>
        </div>
      )}

      <div className="dl-card" style={{overflow:"hidden"}}>
        <div style={{overflowX:"auto"}}>
          <table style={{width:"100%",borderCollapse:"collapse",fontSize:13.5,minWidth:760}}>
            <thead>
              <tr style={{textAlign:"left",color:T.sub,fontSize:11.5,textTransform:"uppercase",letterSpacing:.5}}>
                {["Practice","Contact","Location","Score","Source","Status","Next Follow-Up",""].map(h=>(
                  <th key={h} style={{padding:"13px 14px",borderBottom:`1px solid ${T.border}`,fontWeight:700,position:"sticky",top:0,background:T.card}}>{h}</th>
                ))}
              </tr>
            </thead>
            <tbody>
              {leads.map(l=>(
                <tr key={l.id} className="dl-row" style={{cursor:"pointer"}} onClick={()=>setDetail(l.id)}>
                  <td style={{padding:"12px 14px",borderBottom:`1px solid ${T.border}`}}>
                    <div style={{fontWeight:700,display:"flex",alignItems:"center",gap:7}}>
                      {l.practiceName}
                      {dupes.has(l.id) && <AlertTriangle size={13} color="#f87171"/>}
                    </div>
                    <div style={{fontSize:11.5,color:T.sub}}>{l.specialty} · {l.locations} loc</div>
                  </td>
                  <td style={{padding:"12px 14px",borderBottom:`1px solid ${T.border}`}}>
                    <div style={{fontWeight:600}}>{l.contactName}</div>
                    <div style={{fontSize:11.5,color:T.sub}}>{l.jobTitle}</div>
                  </td>
                  <td style={{padding:"12px 14px",borderBottom:`1px solid ${T.border}`,color:T.sub}}>{l.city}, {l.state}</td>
                  <td style={{padding:"12px 14px",borderBottom:`1px solid ${T.border}`}}>
                    <span className="dl-chip" style={{background:scoreColor(l.score)+"22",color:scoreColor(l.score)}}>{l.score}</span>
                  </td>
                  <td style={{padding:"12px 14px",borderBottom:`1px solid ${T.border}`}}>
                    <span style={{display:"inline-flex",alignItems:"center",gap:5,fontSize:12.5}}><span style={{width:8,height:8,borderRadius:3,background:SOURCE_COLOR[l.source]}}/>{l.source}</span>
                  </td>
                  <td style={{padding:"12px 14px",borderBottom:`1px solid ${T.border}`}}>
                    <span className="dl-chip" style={{background:STAGE_COLOR[l.status]+"22",color:STAGE_COLOR[l.status]}}>{l.status}</span>
                  </td>
                  <td style={{padding:"12px 14px",borderBottom:`1px solid ${T.border}`,fontSize:12.5,color:T.sub}}>
                    {l.nextFollowUp ? <FollowBadge d={l.nextFollowUp} T={T}/> : "—"}
                  </td>
                  <td style={{padding:"12px 14px",borderBottom:`1px solid ${T.border}`,textAlign:"right"}}>
                    <button className="dl-btn" onClick={e=>{e.stopPropagation();setEditing(l);}} style={{background:"transparent",color:T.sub,padding:6}}><Edit3 size={15}/></button>
                  </td>
                </tr>
              ))}
              {leads.length===0 && <tr><td colSpan={8} style={{padding:40,textAlign:"center",color:T.sub}}>No leads match your filters.</td></tr>}
            </tbody>
          </table>
        </div>
      </div>
    </div>
  );
}

function FollowBadge({d,T}){
  const diff=daysFromToday(d);
  const overdue = diff<0;
  const today = diff===0;
  const c = overdue?"#f87171":today?"#fbbf24":T.sub;
  return <span style={{color:c,fontWeight:overdue||today?700:400}}>{overdue?`${-diff}d overdue`:today?"Today":fmtDate(d)}</span>;
}

function Sel({label,v,set,opts,T}){
  return (
    <div>
      <label style={{fontSize:11.5,color:T.sub,fontWeight:600}}>{label}</label>
      <select className="dl-input" value={v} onChange={e=>set(e.target.value)} style={{cursor:"pointer"}}>
        <option value="">All</option>
        {opts.map(o=><option key={o} value={o}>{o}</option>)}
      </select>
    </div>
  );
}

// ============================= PIPELINE (Kanban drag & drop) =============================
function Pipeline({leads,T,moveStage,setDetail}){
  const [dragId,setDragId]=useState(null);
  const [overCol,setOverCol]=useState(null);
  const cols = STAGES;
  const grouped = {}; cols.forEach(c=>grouped[c]=[]);
  leads.forEach(l=>grouped[l.status]?.push(l));

  return (
    <div style={{display:"flex",gap:14,overflowX:"auto",paddingBottom:10}}>
      {cols.map(stage=>(
        <div key={stage}
          className={"kanban-col"+(overCol===stage?" over":"")}
          onDragOver={e=>{e.preventDefault();setOverCol(stage);}}
          onDragLeave={()=>setOverCol(o=>o===stage?null:o)}
          onDrop={()=>{if(dragId!=null)moveStage(dragId,stage);setDragId(null);setOverCol(null);}}
          style={{minWidth:248,width:248,flexShrink:0,background:T.surface2,border:`1px solid ${T.border}`,borderRadius:14,display:"flex",flexDirection:"column",maxHeight:"calc(100vh - 150px)"}}>
          <div style={{padding:"12px 14px",borderBottom:`1px solid ${T.border}`,display:"flex",alignItems:"center",justifyContent:"space-between",position:"sticky",top:0}}>
            <div style={{display:"flex",alignItems:"center",gap:8,fontWeight:700,fontSize:13.5}}>
              <span style={{width:9,height:9,borderRadius:3,background:STAGE_COLOR[stage]}}/>{stage}
            </div>
            <span className="dl-chip" style={{background:T.inputBg,color:T.sub}}>{grouped[stage].length}</span>
          </div>
          <div style={{padding:10,overflowY:"auto",display:"flex",flexDirection:"column",gap:9,flex:1}}>
            {grouped[stage].map(l=>(
              <div key={l.id} draggable className="dl-drag dl-card"
                onDragStart={()=>setDragId(l.id)}
                onDragEnd={()=>{setDragId(null);setOverCol(null);}}
                onClick={()=>setDetail(l.id)}
                style={{padding:"11px 12px",borderRadius:11,opacity:dragId===l.id?.4:1,borderLeft:`3px solid ${STAGE_COLOR[stage]}`}}>
                <div style={{fontWeight:700,fontSize:13}}>{l.practiceName}</div>
                <div style={{fontSize:11.5,color:T.sub,marginTop:2}}>{l.contactName}</div>
                <div style={{display:"flex",alignItems:"center",justifyContent:"space-between",marginTop:9}}>
                  <span style={{fontSize:11,color:T.sub}}>{l.city}, {l.state}</span>
                  <span className="dl-chip" style={{background:scoreColor(l.score)+"22",color:scoreColor(l.score)}}>{l.score}</span>
                </div>
              </div>
            ))}
            {grouped[stage].length===0 && <div style={{fontSize:12,color:T.sub,textAlign:"center",padding:"18px 0",opacity:.6}}>Drop leads here</div>}
          </div>
        </div>
      ))}
    </div>
  );
}

// ============================= FOLLOW-UPS (tasks + calendar) =============================
function FollowUps({leads,T,setDetail}){
  const [tab,setTab]=useState("list");
  const withFu = leads.filter(l=>l.nextFollowUp).sort((a,b)=>a.nextFollowUp.localeCompare(b.nextFollowUp));
  const overdue = withFu.filter(l=>daysFromToday(l.nextFollowUp)<0);
  const today = withFu.filter(l=>daysFromToday(l.nextFollowUp)===0);
  const upcoming = withFu.filter(l=>daysFromToday(l.nextFollowUp)>0);

  return (
    <div>
      <div style={{display:"grid",gridTemplateColumns:"repeat(auto-fit,minmax(150px,1fr))",gap:12,marginBottom:16}}>
        {[["Overdue",overdue.length,"#f87171",AlertTriangle],["Due Today",today.length,"#fbbf24",Clock],["Upcoming",upcoming.length,T.accent,CalendarClock]].map(([l,n,c,Icon])=>(
          <div key={l} className="dl-card" style={{padding:"15px 17px",display:"flex",alignItems:"center",gap:13}}>
            <div style={{width:38,height:38,borderRadius:11,background:c+"22",display:"grid",placeItems:"center"}}><Icon size={19} color={c}/></div>
            <div><div style={{fontFamily:"Sora",fontSize:24,fontWeight:800}}>{n}</div><div style={{fontSize:12,color:T.sub,fontWeight:600}}>{l}</div></div>
          </div>
        ))}
      </div>

      <div style={{display:"flex",gap:8,marginBottom:14}}>
        {[["list","Task List"],["calendar","Calendar"]].map(([k,lab])=>(
          <button key={k} className="dl-btn" onClick={()=>setTab(k)} style={{background:tab===k?T.accent+"22":T.inputBg,color:tab===k?T.accent:T.sub,padding:"8px 16px",borderRadius:9,fontSize:13,border:`1px solid ${T.border}`}}>{lab}</button>
        ))}
      </div>

      {tab==="list" ? (
        <div style={{display:"flex",flexDirection:"column",gap:18}}>
          {[["Overdue",overdue,"#f87171"],["Due Today",today,"#fbbf24"],["Upcoming",upcoming,T.accent]].map(([title,list,c])=> list.length>0 && (
            <div key={title}>
              <div style={{fontSize:13,fontWeight:700,color:c,marginBottom:9,display:"flex",alignItems:"center",gap:7}}><span style={{width:8,height:8,borderRadius:3,background:c}}/>{title} ({list.length})</div>
              <div className="dl-card" style={{overflow:"hidden"}}>
                {list.map((l,i)=>(
                  <div key={l.id} className="dl-row" onClick={()=>setDetail(l.id)} style={{cursor:"pointer",padding:"13px 16px",display:"flex",alignItems:"center",gap:14,borderBottom:i<list.length-1?`1px solid ${T.border}`:"none"}}>
                    <div style={{flex:1}}>
                      <div style={{fontWeight:700,fontSize:13.5}}>{l.practiceName}</div>
                      <div style={{fontSize:12,color:T.sub}}>{l.contactName} · {l.jobTitle}</div>
                    </div>
                    <span className="dl-chip" style={{background:STAGE_COLOR[l.status]+"22",color:STAGE_COLOR[l.status]}}>{l.status}</span>
                    <div style={{minWidth:96,textAlign:"right"}}><FollowBadge d={l.nextFollowUp} T={T}/></div>
                    <ChevronRight size={16} color={T.sub}/>
                  </div>
                ))}
              </div>
            </div>
          ))}
          {withFu.length===0 && <div className="dl-card" style={{padding:40,textAlign:"center",color:T.sub}}>No follow-ups scheduled.</div>}
        </div>
      ) : <CalendarView leads={withFu} T={T} setDetail={setDetail}/>}
    </div>
  );
}

function CalendarView({leads,T,setDetail}){
  // June 2026 month grid
  const year=2026,month=5; // 0-indexed June
  const first=new Date(year,month,1).getDay();
  const days=new Date(year,month+1,0).getDate();
  const byDay={};
  leads.forEach(l=>{const d=new Date(l.nextFollowUp+"T00:00:00");if(d.getFullYear()===year&&d.getMonth()===month){(byDay[d.getDate()]=byDay[d.getDate()]||[]).push(l);}});
  const cells=[]; for(let i=0;i<first;i++)cells.push(null); for(let d=1;d<=days;d++)cells.push(d);
  return (
    <div className="dl-card" style={{padding:18}}>
      <h3 style={{fontSize:15,fontWeight:700,marginBottom:14}}>June 2026</h3>
      <div style={{display:"grid",gridTemplateColumns:"repeat(7,1fr)",gap:6}}>
        {["Sun","Mon","Tue","Wed","Thu","Fri","Sat"].map(d=><div key={d} style={{fontSize:11,fontWeight:700,color:T.sub,textAlign:"center",padding:4}}>{d}</div>)}
        {cells.map((d,i)=>(
          <div key={i} style={{minHeight:74,borderRadius:9,border:`1px solid ${T.border}`,padding:6,background:d===2?T.accent+"14":T.surface2,opacity:d?1:.3}}>
            {d && <div style={{fontSize:11.5,fontWeight:700,color:d===2?T.accent:T.sub,marginBottom:3}}>{d}</div>}
            {(byDay[d]||[]).slice(0,3).map(l=>(
              <div key={l.id} onClick={()=>setDetail(l.id)} title={l.practiceName} style={{cursor:"pointer",fontSize:10,fontWeight:600,padding:"2px 5px",borderRadius:5,marginBottom:2,background:STAGE_COLOR[l.status]+"33",color:STAGE_COLOR[l.status],whiteSpace:"nowrap",overflow:"hidden",textOverflow:"ellipsis"}}>{l.practiceName}</div>
            ))}
            {(byDay[d]||[]).length>3 && <div style={{fontSize:10,color:T.sub}}>+{byDay[d].length-3} more</div>}
          </div>
        ))}
      </div>
    </div>
  );
}

// ============================= REPORTS =============================
function Reports({leads,T}){
  const total=leads.length;
  const won=leads.filter(l=>l.status==="Won").length;
  const lost=leads.filter(l=>l.status==="Lost").length;
  const active=total-won-lost;

  // source performance
  const srcPerf = SOURCES.map(s=>{
    const ls=leads.filter(l=>l.source===s);
    const w=ls.filter(l=>l.status==="Won").length;
    return {s,total:ls.length,won:w,rate:ls.length?Math.round(w/ls.length*100):0};
  }).filter(r=>r.total>0).sort((a,b)=>b.total-a.total);

  // state report
  const stateRep = Object.entries(leads.reduce((m,l)=>{m[l.state]=m[l.state]||{t:0,w:0};m[l.state].t++;if(l.status==="Won")m[l.state].w++;return m;},{})).map(([st,v])=>({st,...v})).sort((a,b)=>b.t-a.t);

  // funnel
  const funnel = STAGES.filter(s=>s!=="Lost").map(s=>({s,n:leads.filter(l=>l.status===s).length}));
  const maxFun=Math.max(1,...funnel.map(f=>f.n));

  const Card=({title,icon:Icon,children})=>(
    <div className="dl-card" style={{padding:"18px 20px"}}>
      <h3 style={{fontSize:14.5,fontWeight:700,marginBottom:15,display:"flex",alignItems:"center",gap:8}}><Icon size={16} color={T.accent}/>{title}</h3>
      {children}
    </div>
  );

  return (
    <div style={{display:"flex",flexDirection:"column",gap:16}}>
      <div style={{display:"grid",gridTemplateColumns:"repeat(auto-fit,minmax(180px,1fr))",gap:14}}>
        {[["Total Acquired",total,T.accent],["Closed Won",won,"#34d399"],["Closed Lost",lost,"#f87171"],["Win Rate",(won+lost)?Math.round(won/(won+lost)*100)+"%":"0%",T.accent2]].map(([l,v,c])=>(
          <div key={l} className="dl-card" style={{padding:"16px 18px"}}>
            <div style={{fontSize:12.5,color:T.sub,fontWeight:600}}>{l}</div>
            <div style={{fontFamily:"Sora",fontSize:28,fontWeight:800,color:c,marginTop:6}}>{v}</div>
          </div>
        ))}
      </div>

      <div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:16}}>
        <Card title="Sales Conversion Funnel" icon={TrendingUp}>
          {funnel.map(f=>(
            <div key={f.s} style={{marginBottom:11}}>
              <div style={{display:"flex",justifyContent:"space-between",fontSize:12.5,marginBottom:4}}><span style={{color:T.sub,fontWeight:600}}>{f.s}</span><b>{f.n}</b></div>
              <div style={{height:10,background:T.inputBg,borderRadius:6,overflow:"hidden"}}><div style={{width:`${f.n/maxFun*100}%`,height:"100%",background:STAGE_COLOR[f.s],borderRadius:6}}/></div>
            </div>
          ))}
        </Card>

        <Card title="Source Performance Report" icon={Globe}>
          <table style={{width:"100%",borderCollapse:"collapse",fontSize:13}}>
            <thead><tr style={{color:T.sub,fontSize:11,textTransform:"uppercase"}}>
              <th style={{textAlign:"left",padding:"4px 4px 10px"}}>Source</th><th style={{padding:"4px 4px 10px"}}>Leads</th><th style={{padding:"4px 4px 10px"}}>Won</th><th style={{padding:"4px 4px 10px"}}>Rate</th>
            </tr></thead>
            <tbody>{srcPerf.map(r=>(
              <tr key={r.s} style={{borderTop:`1px solid ${T.border}`}}>
                <td style={{padding:"9px 4px",fontWeight:600}}><span style={{display:"inline-flex",alignItems:"center",gap:6}}><span style={{width:8,height:8,borderRadius:3,background:SOURCE_COLOR[r.s]}}/>{r.s}</span></td>
                <td style={{padding:"9px 4px",textAlign:"center"}}>{r.total}</td>
                <td style={{padding:"9px 4px",textAlign:"center",color:"#34d399",fontWeight:700}}>{r.won}</td>
                <td style={{padding:"9px 4px",textAlign:"center"}}><span className="dl-chip" style={{background:scoreColor(r.rate)+"22",color:scoreColor(r.rate)}}>{r.rate}%</span></td>
              </tr>
            ))}</tbody>
          </table>
        </Card>
      </div>

      <Card title="State-wise Lead Report" icon={MapPin}>
        <div style={{display:"grid",gridTemplateColumns:"repeat(auto-fill,minmax(120px,1fr))",gap:10}}>
          {stateRep.map(r=>(
            <div key={r.st} style={{padding:"11px 13px",borderRadius:10,background:T.surface2,border:`1px solid ${T.border}`}}>
              <div style={{fontWeight:800,fontFamily:"Sora",fontSize:17}}>{r.st}</div>
              <div style={{fontSize:11.5,color:T.sub,marginTop:3}}>{r.t} leads · <span style={{color:"#34d399",fontWeight:700}}>{r.w} won</span></div>
            </div>
          ))}
        </div>
      </Card>
    </div>
  );
}

// ============================= LEAD MODAL (add / edit) =============================
const EMPTY={practiceName:"",contactName:"",jobTitle:"",website:"",facebook:"",instagram:"",linkedin:"",email:"",phone:"",state:"",city:"",locations:1,dentists:1,specialty:SPECIALTIES[0],source:SOURCES[0],score:50,status:"New",lastContact:"",nextFollowUp:"",notes:""};

function LeadModal({lead,onSave,onClose,T}){
  const [f,setF]=useState(lead?{...lead}:{...EMPTY});
  const set=(k,v)=>setF(p=>({...p,[k]:v}));
  const Field=({label,k,type="text",full})=>(
    <div style={{gridColumn:full?"1/-1":"auto"}}>
      <label style={{fontSize:11.5,color:T.sub,fontWeight:600,display:"block",marginBottom:4}}>{label}</label>
      {type==="textarea"
        ? <textarea className="dl-input" rows={3} value={f[k]} onChange={e=>set(k,e.target.value)} style={{resize:"vertical"}}/>
        : <input className="dl-input" type={type} value={f[k]} onChange={e=>set(k,type==="number"?+e.target.value:e.target.value)}/>}
    </div>
  );
  return (
    <Overlay onClose={onClose}>
      <div className="dl-card dl-fade" onClick={e=>e.stopPropagation()} style={{width:"min(720px,94vw)",maxHeight:"90vh",overflow:"hidden",display:"flex",flexDirection:"column"}}>
        <div style={{padding:"18px 22px",borderBottom:`1px solid ${T.border}`,display:"flex",justifyContent:"space-between",alignItems:"center"}}>
          <h2 style={{fontSize:17,fontWeight:800}}>{lead?"Edit Lead":"New Lead"}</h2>
          <button className="dl-btn" onClick={onClose} style={{background:T.inputBg,color:T.sub,padding:8,borderRadius:9}}><X size={17}/></button>
        </div>
        <div style={{padding:22,overflowY:"auto",display:"grid",gridTemplateColumns:"1fr 1fr",gap:14}}>
          <Field label="Practice Name *" k="practiceName" full/>
          <Field label="Contact Name" k="contactName"/>
          <Field label="Job Title" k="jobTitle"/>
          <Field label="Email" k="email" type="email"/>
          <Field label="Phone" k="phone"/>
          <Field label="City" k="city"/>
          <Field label="State" k="state"/>
          <Field label="Website" k="website"/>
          <Field label="Facebook URL" k="facebook"/>
          <Field label="Instagram URL" k="instagram"/>
          <Field label="LinkedIn URL" k="linkedin"/>
          <Field label="Number of Locations" k="locations" type="number"/>
          <Field label="Number of Dentists" k="dentists" type="number"/>
          <div>
            <label style={{fontSize:11.5,color:T.sub,fontWeight:600,display:"block",marginBottom:4}}>Specialty</label>
            <select className="dl-input" value={f.specialty} onChange={e=>set("specialty",e.target.value)}>{SPECIALTIES.map(s=><option key={s}>{s}</option>)}</select>
          </div>
          <div>
            <label style={{fontSize:11.5,color:T.sub,fontWeight:600,display:"block",marginBottom:4}}>Lead Source</label>
            <select className="dl-input" value={f.source} onChange={e=>set("source",e.target.value)}>{SOURCES.map(s=><option key={s}>{s}</option>)}</select>
          </div>
          <div>
            <label style={{fontSize:11.5,color:T.sub,fontWeight:600,display:"block",marginBottom:4}}>Status</label>
            <select className="dl-input" value={f.status} onChange={e=>set("status",e.target.value)}>{STAGES.map(s=><option key={s}>{s}</option>)}</select>
          </div>
          <div>
            <label style={{fontSize:11.5,color:T.sub,fontWeight:600,display:"block",marginBottom:4}}>Lead Score: <b style={{color:scoreColor(f.score)}}>{f.score}</b></label>
            <input type="range" min={0} max={100} value={f.score} onChange={e=>set("score",+e.target.value)} style={{width:"100%",accentColor:scoreColor(f.score)}}/>
          </div>
          <Field label="Last Contact Date" k="lastContact" type="date"/>
          <Field label="Next Follow-Up Date" k="nextFollowUp" type="date"/>
          <Field label="Notes" k="notes" type="textarea" full/>
        </div>
        <div style={{padding:"15px 22px",borderTop:`1px solid ${T.border}`,display:"flex",justifyContent:"flex-end",gap:10}}>
          <button className="dl-btn" onClick={onClose} style={{background:T.inputBg,color:T.text,padding:"10px 18px",borderRadius:10,border:`1px solid ${T.border}`}}>Cancel</button>
          <button className="dl-btn" disabled={!f.practiceName.trim()} onClick={()=>onSave(f)} style={{background:`linear-gradient(135deg,${T.accent},${T.accent2})`,color:"#04121a",padding:"10px 20px",borderRadius:10,opacity:f.practiceName.trim()?1:.5}}><Check size={16}/>Save Lead</button>
        </div>
      </div>
    </Overlay>
  );
}

function Overlay({children,onClose}){
  return <div onClick={onClose} style={{position:"fixed",inset:0,background:"rgba(2,6,14,.6)",backdropFilter:"blur(4px)",display:"grid",placeItems:"center",zIndex:100,padding:16}}>{children}</div>;
}

// ============================= DETAIL DRAWER (AI + timeline + contact log) =============================
const AI_ACTIONS=["Analyze Practice","Generate Outreach Email","Generate LinkedIn Message","Generate Facebook Message","Generate Follow-Up Email","Summarize Notes"];

function DetailDrawer({lead,T,onClose,onEdit,onDelete,isDupe,onUpdate}){
  const [tab,setTab]=useState("overview");
  const [ai,setAi]=useState(null);        // {kind,text}
  const [loading,setLoading]=useState(null);
  const [copied,setCopied]=useState(false);
  const [logText,setLogText]=useState("");

  const runAI=(kind)=>{
    setLoading(kind); setAi(null);
    setTimeout(()=>{ setAi({kind,text:aiGenerate(kind,lead)}); setLoading(null); },650);
  };
  const copy=()=>{ navigator.clipboard?.writeText(ai.text); setCopied(true); setTimeout(()=>setCopied(false),1400); };
  const addLog=()=>{
    if(!logText.trim())return;
    onUpdate({...lead,_activity:[{t:TODAY,msg:logText.trim(),type:"log"},...(lead._activity||[])]});
    setLogText("");
  };

  const Social=({url,Icon,label,c})=> url ? <a href={url} target="_blank" rel="noreferrer" style={{display:"flex",alignItems:"center",gap:7,fontSize:12.5,color:c,textDecoration:"none",fontWeight:600}}><Icon size={15}/>{label}<ArrowUpRight size={12}/></a> : null;

  return (
    <div onClick={onClose} style={{position:"fixed",inset:0,background:"rgba(2,6,14,.5)",backdropFilter:"blur(3px)",zIndex:100,display:"flex",justifyContent:"flex-end"}}>
      <div className="dl-fade" onClick={e=>e.stopPropagation()} style={{width:"min(560px,100vw)",height:"100%",background:T.surface,borderLeft:`1px solid ${T.border}`,display:"flex",flexDirection:"column",boxShadow:T.shadow}}>
        {/* header */}
        <div style={{padding:"20px 22px",borderBottom:`1px solid ${T.border}`}}>
          <div style={{display:"flex",justifyContent:"space-between",alignItems:"flex-start",gap:12}}>
            <div>
              <div style={{display:"flex",alignItems:"center",gap:9,flexWrap:"wrap"}}>
                <h2 style={{fontSize:19,fontWeight:800}}>{lead.practiceName}</h2>
                {isDupe && <span className="dl-chip" style={{background:"#f87171"+"22",color:"#f87171",display:"inline-flex",alignItems:"center",gap:4}}><AlertTriangle size={12}/>Possible duplicate</span>}
              </div>
              <div style={{fontSize:13,color:T.sub,marginTop:3}}>{lead.contactName} · {lead.jobTitle}</div>
            </div>
            <button className="dl-btn" onClick={onClose} style={{background:T.inputBg,color:T.sub,padding:8,borderRadius:9}}><X size={17}/></button>
          </div>
          <div style={{display:"flex",gap:8,marginTop:14,flexWrap:"wrap"}}>
            <span className="dl-chip" style={{background:STAGE_COLOR[lead.status]+"22",color:STAGE_COLOR[lead.status]}}>{lead.status}</span>
            <span className="dl-chip" style={{background:scoreColor(lead.score)+"22",color:scoreColor(lead.score)}}>Score {lead.score}</span>
            <span className="dl-chip" style={{background:SOURCE_COLOR[lead.source]+"22",color:SOURCE_COLOR[lead.source]}}>{lead.source}</span>
            <button className="dl-btn" onClick={()=>onEdit(lead)} style={{marginLeft:"auto",background:T.inputBg,color:T.text,padding:"6px 12px",borderRadius:9,fontSize:12.5,border:`1px solid ${T.border}`}}><Edit3 size={13}/>Edit</button>
            <button className="dl-btn" onClick={()=>onDelete(lead.id)} style={{background:"#f8717118",color:"#f87171",padding:"6px 11px",borderRadius:9,fontSize:12.5}}><Trash2 size={13}/></button>
          </div>
        </div>

        {/* tabs */}
        <div style={{display:"flex",gap:4,padding:"10px 16px 0",borderBottom:`1px solid ${T.border}`}}>
          {[["overview","Overview"],["ai","AI Studio"],["timeline","Activity"]].map(([k,lab])=>(
            <button key={k} className="dl-btn" onClick={()=>setTab(k)} style={{background:"transparent",color:tab===k?T.accent:T.sub,padding:"9px 14px",fontSize:13,borderBottom:tab===k?`2px solid ${T.accent}`:"2px solid transparent",borderRadius:0}}>{lab}</button>
          ))}
        </div>

        <div style={{overflowY:"auto",padding:22,flex:1}}>
          {tab==="overview" && (
            <div style={{display:"flex",flexDirection:"column",gap:18}}>
              <Section T={T} title="Contact">
                <Row T={T} Icon={Mail} v={lead.email}/>
                <Row T={T} Icon={Phone} v={lead.phone}/>
                <Row T={T} Icon={MapPin} v={`${lead.city}, ${lead.state}`}/>
                <Row T={T} Icon={Globe} v={lead.website} link/>
              </Section>
              <Section T={T} title="Social">
                <div style={{display:"flex",gap:18,flexWrap:"wrap"}}>
                  <Social url={lead.facebook} Icon={Facebook} label="Facebook" c="#1877f2"/>
                  <Social url={lead.instagram} Icon={Instagram} label="Instagram" c="#e1306c"/>
                  <Social url={lead.linkedin} Icon={Linkedin} label="LinkedIn" c="#0a66c2"/>
                  {!lead.facebook&&!lead.instagram&&!lead.linkedin&&<span style={{fontSize:12.5,color:T.sub}}>No social profiles on file.</span>}
                </div>
              </Section>
              <Section T={T} title="Practice Details">
                <div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:10}}>
                  <Stat T={T} l="Specialty" v={lead.specialty}/>
                  <Stat T={T} l="Locations" v={lead.locations}/>
                  <Stat T={T} l="Dentists" v={lead.dentists}/>
                  <Stat T={T} l="Last Contact" v={fmtDate(lead.lastContact)}/>
                  <Stat T={T} l="Next Follow-Up" v={fmtDate(lead.nextFollowUp)}/>
                </div>
              </Section>
              <Section T={T} title="Notes">
                <div style={{fontSize:13,color:T.text,lineHeight:1.6,background:T.inputBg,padding:"12px 14px",borderRadius:10}}>{lead.notes||"No notes recorded."}</div>
              </Section>
            </div>
          )}

          {tab==="ai" && (
            <div>
              <div style={{display:"flex",alignItems:"center",gap:8,marginBottom:12}}><Sparkles size={16} color={T.accent}/><b style={{fontSize:14}}>AI Studio</b></div>
              <div style={{display:"grid",gridTemplateColumns:"1fr 1fr",gap:9,marginBottom:18}}>
                {AI_ACTIONS.map(a=>(
                  <button key={a} className="dl-btn" onClick={()=>runAI(a)} disabled={loading} style={{justifyContent:"flex-start",background:ai?.kind===a?T.accent+"22":T.inputBg,color:ai?.kind===a?T.accent:T.text,padding:"11px 13px",borderRadius:10,border:`1px solid ${T.border}`,fontSize:12.5,textAlign:"left"}}>
                    {loading===a ? <span style={{width:14,height:14,border:`2px solid ${T.accent}`,borderTopColor:"transparent",borderRadius:"50%",animation:"spin .7s linear infinite",display:"inline-block"}}/> : <Sparkles size={14} color={T.accent}/>}
                    {a}
                  </button>
                ))}
              </div>
              {ai && (
                <div className="dl-card dl-fade" style={{padding:16,background:T.inputBg}}>
                  <div style={{display:"flex",justifyContent:"space-between",alignItems:"center",marginBottom:10}}>
                    <b style={{fontSize:13,color:T.accent}}>{ai.kind}</b>
                    <button className="dl-btn" onClick={copy} style={{background:T.surface,color:copied?T.accent2:T.sub,padding:"6px 11px",borderRadius:8,fontSize:12,border:`1px solid ${T.border}`}}>{copied?<Check size={13}/>:<Copy size={13}/>}{copied?"Copied":"Copy"}</button>
                  </div>
                  <pre style={{whiteSpace:"pre-wrap",fontFamily:"inherit",fontSize:12.8,lineHeight:1.6,color:T.text,margin:0}}>{ai.text}</pre>
                </div>
              )}
              {!ai && !loading && <div style={{fontSize:12.5,color:T.sub,textAlign:"center",padding:"20px 0"}}>Pick an action above to generate AI content for this lead.</div>}
            </div>
          )}

          {tab==="timeline" && (
            <div>
              <Section T={T} title="Log a contact">
                <div style={{display:"flex",gap:8}}>
                  <input className="dl-input" placeholder="e.g. Left voicemail, sent proposal…" value={logText} onChange={e=>setLogText(e.target.value)} onKeyDown={e=>e.key==="Enter"&&addLog()}/>
                  <button className="dl-btn" onClick={addLog} style={{background:T.accent,color:"#04121a",padding:"0 14px",borderRadius:10}}><Plus size={16}/></button>
                </div>
              </Section>
              <div style={{marginTop:18}}>
                <div style={{fontSize:11.5,color:T.sub,fontWeight:700,textTransform:"uppercase",letterSpacing:.5,marginBottom:12}}>Activity Timeline</div>
                {(lead._activity&&lead._activity.length)?(
                  <div style={{position:"relative",paddingLeft:20}}>
                    <div style={{position:"absolute",left:5,top:4,bottom:4,width:2,background:T.border}}/>
                    {lead._activity.map((a,i)=>(
                      <div key={i} style={{position:"relative",marginBottom:16}}>
                        <span style={{position:"absolute",left:-19,top:3,width:10,height:10,borderRadius:"50%",background:a.type==="log"?T.accent2:T.accent,border:`2px solid ${T.surface}`}}/>
                        <div style={{fontSize:13,fontWeight:600}}>{a.msg}</div>
                        <div style={{fontSize:11.5,color:T.sub,marginTop:1}}>{fmtDate(a.t)}{a.type==="log"&&" · Contact log"}</div>
                      </div>
                    ))}
                  </div>
                ):<div style={{fontSize:12.5,color:T.sub}}>No activity yet. Move this lead in the pipeline or log a contact to start the timeline.</div>}
              </div>
            </div>
          )}
        </div>
      </div>
    </div>
  );
}

function Section({title,children,T}){return <div><div style={{fontSize:11.5,color:T.sub,fontWeight:700,textTransform:"uppercase",letterSpacing:.5,marginBottom:10}}>{title}</div>{children}</div>;}
function Row({Icon,v,link,T}){return <div style={{display:"flex",alignItems:"center",gap:10,marginBottom:9,fontSize:13}}><Icon size={15} color={T.sub}/>{link&&v?<a href={v} target="_blank" rel="noreferrer" style={{color:T.accent,textDecoration:"none"}}>{v}</a>:<span>{v||"—"}</span>}</div>;}
function Stat({l,v,T}){return <div style={{background:T.inputBg,padding:"10px 12px",borderRadius:10}}><div style={{fontSize:11,color:T.sub,fontWeight:600}}>{l}</div><div style={{fontSize:13.5,fontWeight:700,marginTop:2}}>{v}</div></div>;}

// ============================= LEAD DISCOVERY MODULE =============================
// Searches public dental-practice listings by state + specialty, extracts contact &
// social profiles, auto-scores, flags multi-location groups, and saves into the CRM.
//
// ⚙️ PRODUCTION WIRING: replace `discoverPractices()` below with a server call to your
// data source — e.g. fetch("/api/discovery?state=..&specialty=..") backed by the Google
// Places API, a business-directory provider, and social-profile lookups. The component
// already handles loading, scoring, dedupe, outreach and save, so only the fetch changes.

const US_STATES = ["AL","AK","AZ","AR","CA","CO","CT","DE","FL","GA","HI","ID","IL","IN","IA","KS","KY","LA","ME","MD","MA","MI","MN","MS","MO","MT","NE","NV","NH","NJ","NM","NY","NC","ND","OH","OK","OR","PA","RI","SC","SD","TN","TX","UT","VT","VA","WA","WV","WI","WY"];
const STATE_CITIES = {
  CA:["Los Angeles","San Diego","San Jose","San Francisco","Sacramento","Fresno","Long Beach"],
  TX:["Houston","Dallas","Austin","San Antonio","Fort Worth","El Paso"],
  FL:["Miami","Orlando","Tampa","Jacksonville","Fort Lauderdale"],
  NY:["New York","Buffalo","Rochester","Albany","Syracuse"],
  IL:["Chicago","Aurora","Naperville","Springfield"],
  WA:["Seattle","Spokane","Tacoma","Bellevue"],
  GA:["Atlanta","Savannah","Augusta","Athens"],
  CO:["Denver","Boulder","Colorado Springs","Fort Collins"],
  AZ:["Phoenix","Tucson","Mesa","Scottsdale"],
  MA:["Boston","Cambridge","Worcester"],
  PA:["Philadelphia","Pittsburgh","Allentown"],
  OH:["Columbus","Cleveland","Cincinnati"],
  NC:["Charlotte","Raleigh","Greensboro"],
  TN:["Nashville","Memphis","Knoxville"],
  MI:["Detroit","Grand Rapids","Ann Arbor"],
};
const DISC_PRE = ["Bright","Pearl","Summit","Lakeside","Riverstone","Parkview","Cedar","Maplewood","Sunrise","Coastal","Valley","Premier","Gentle","Family","Modern","Smile","Aspen","Willow","Harbor","Oakridge","Pinecrest","Meadow","Crystal","Radiant","Blue Sky","Evergreen","Capital","Metro","Grandview","Heritage","Lighthouse","Cornerstone"];
const DISC_SUF = {
  "General Dentistry":["Dental","Dental Care","Family Dentistry","Dental Group"],
  "Orthodontics":["Orthodontics","Braces & Aligners","Orthodontic Studio"],
  "Pediatric Dentistry":["Pediatric Dentistry","Kids Dental","Children's Dental"],
  "Periodontics":["Periodontics","Gum & Implant Center","Periodontal Associates"],
  "Endodontics":["Endodontics","Root Canal Specialists","Endodontic Group"],
  "Oral Surgery":["Oral Surgery","Oral & Maxillofacial Surgery","Surgical Arts"],
  "Prosthodontics":["Prosthodontics","Implant & Restorative","Prosthodontic Center"],
  "Cosmetic Dentistry":["Cosmetic Dentistry","Smile Design Studio","Aesthetic Dental"],
};
const FN=["James","Mary","Robert","Patricia","Michael","Jennifer","David","Linda","Priya","Wei","Carlos","Aisha","Raj","Maria","Susan","Daniel","Emily","Mark","Nancy","Joseph"];
const LN=["Smith","Johnson","Garcia","Martinez","Lee","Patel","Nguyen","Kim","Chen","Brown","Davis","Lopez","Wilson","Cohen","Murphy","Shah","Reed","Bailey","Wong","Thompson"];

// Deterministic PRNG so the same search returns stable results within a session
function makeRng(seedStr){
  let h=2166136261;
  for(let i=0;i<seedStr.length;i++){h^=seedStr.charCodeAt(i);h=Math.imul(h,16777619);}
  return ()=>{h^=h<<13;h^=h>>>17;h^=h<<5;return ((h>>>0)%100000)/100000;};
}

// Auto lead-scoring model: rewards multi-location, digital presence, contact completeness
function scoreDiscovered(p){
  let s=30;
  s += Math.min(p.locations,5)*7;          // footprint
  s += Math.min(p.dentists,12);            // size
  if(p.website) s+=8;
  if(p.facebook) s+=5;
  if(p.instagram) s+=5;
  if(p.linkedin) s+=7;
  if(p.email) s+=6;
  if(p.phone) s+=4;
  return Math.max(20,Math.min(99,Math.round(s)));
}

function outreachFor(p){
  const fn=(p.contactName||"there").replace(/^Dr\.\s*/,"").split(" ")[0];
  const multi = p.locations>1;
  return `Hi ${fn}, I came across ${p.practiceName} while researching top ${p.specialty.toLowerCase()} practices in ${p.city}, ${p.state}. ${multi?`Scaling patient acquisition across ${p.locations} locations`:"Growing new-patient volume"} is exactly what we help dental practices with. Open to a quick 15-min call this week?`;
}

// 🔌 SWAP THIS for a real API call in production.
function discoverPractices({state,specialty,minLocations}){
  const rng = makeRng(state+"|"+specialty+"|"+minLocations);
  const cities = STATE_CITIES[state] || ["Downtown","Midtown","Westside","Eastgate","Northpark"];
  const specs = specialty==="All Specialties" ? Object.keys(DISC_SUF) : [specialty];
  const count = 8 + Math.floor(rng()*7); // 8–14 results
  const used=new Set(); const out=[];
  for(let i=0;i<count;i++){
    const spec = specs[Math.floor(rng()*specs.length)];
    let name;
    do{ name = `${DISC_PRE[Math.floor(rng()*DISC_PRE.length)]} ${DISC_SUF[spec][Math.floor(rng()*DISC_SUF[spec].length)]}`; }
    while(used.has(name)); used.add(name);
    const slug = name.toLowerCase().replace(/[^a-z0-9]/g,"");
    const city = cities[Math.floor(rng()*cities.length)];
    const locations = Math.max(1, minLocations, [1,1,1,2,2,3,4,6][Math.floor(rng()*8)]);
    const dentists = locations + Math.floor(rng()*locations*3)+1;
    const fn=FN[Math.floor(rng()*FN.length)], ln=LN[Math.floor(rng()*LN.length)];
    const hasFb=rng()>0.25, hasIg=rng()>0.3, hasLi=rng()>0.45, hasEmail=rng()>0.2, hasPhone=rng()>0.1;
    const p={
      _discoveryId: slug+i,
      practiceName:name,
      contactName: `Dr. ${fn} ${ln}`,
      jobTitle:"Owner / Dentist",
      website:`https://www.${slug}.com`,
      facebook: hasFb?`https://facebook.com/${slug}`:"",
      instagram: hasIg?`https://instagram.com/${slug}`:"",
      linkedin: hasLi?`https://linkedin.com/company/${slug}`:"",
      email: hasEmail?`info@${slug}.com`:"",
      phone: hasPhone?`(${200+Math.floor(rng()*700)}) ${200+Math.floor(rng()*700)}-${1000+Math.floor(rng()*8999)}`:"",
      state, city, locations, dentists,
      specialty:spec,
      source:"Discovery",
    };
    p.score = scoreDiscovered(p);
    p.outreach = outreachFor(p);
    out.push(p);
  }
  return out.sort((a,b)=>b.score-a.score);
}

function Discovery({T,importLead,existing}){
  const [state,setState]=useState("CA");
  const [specialty,setSpecialty]=useState("All Specialties");
  const [minLoc,setMinLoc]=useState(1);
  const [loading,setLoading]=useState(false);
  const [results,setResults]=useState(null);
  const [saved,setSaved]=useState({});     // _discoveryId -> "added"|"duplicate"
  const [openMsg,setOpenMsg]=useState(null);

  const existsKey = useMemo(()=>new Set(existing.map(l=>(l.email||"").toLowerCase()||l.practiceName.toLowerCase().replace(/[^a-z]/g,""))),[existing]);
  const isKnown=(p)=> existsKey.has((p.email||"").toLowerCase()||p.practiceName.toLowerCase().replace(/[^a-z]/g,""));

  const runSearch=()=>{
    setLoading(true); setResults(null); setSaved({});
    // simulates network + extraction latency; replace with real fetch
    setTimeout(()=>{ setResults(discoverPractices({state,specialty,minLocations:+minLoc})); setLoading(false); },900);
  };

  const saveOne=(p)=>{
    const r=importLead(p);
    setSaved(s=>({...s,[p._discoveryId]:r}));
  };
  const saveAll=()=>{
    const next={...saved};
    results.forEach(p=>{ if(!next[p._discoveryId]) next[p._discoveryId]=importLead(p); });
    setSaved(next);
  };

  const newCount = results? results.filter(p=>!saved[p._discoveryId] && !isKnown(p)).length : 0;
  const multiCount = results? results.filter(p=>p.locations>1).length : 0;

  return (
    <div>
      {/* search panel */}
      <div className="dl-card" style={{padding:"20px 22px",marginBottom:18,background:`linear-gradient(135deg,${T.accent}10,transparent)`}}>
        <div style={{display:"flex",alignItems:"center",gap:10,marginBottom:4}}>
          <div style={{width:34,height:34,borderRadius:10,background:T.accent+"22",display:"grid",placeItems:"center"}}><Radar size={18} color={T.accent}/></div>
          <div>
            <h2 style={{fontSize:17,fontWeight:800}}>Discover New Leads</h2>
            <div style={{fontSize:12.5,color:T.sub}}>Find dental practices from public business listings & social profiles.</div>
          </div>
        </div>
        <div style={{display:"grid",gridTemplateColumns:"1fr 1fr 0.7fr auto",gap:12,alignItems:"end",marginTop:16}}>
          <div>
            <label style={{fontSize:11.5,color:T.sub,fontWeight:600,display:"block",marginBottom:4}}>U.S. State</label>
            <select className="dl-input" value={state} onChange={e=>setState(e.target.value)} style={{cursor:"pointer"}}>
              {US_STATES.map(s=><option key={s} value={s}>{s}</option>)}
            </select>
          </div>
          <div>
            <label style={{fontSize:11.5,color:T.sub,fontWeight:600,display:"block",marginBottom:4}}>Specialty</label>
            <select className="dl-input" value={specialty} onChange={e=>setSpecialty(e.target.value)} style={{cursor:"pointer"}}>
              <option>All Specialties</option>
              {SPECIALTIES.map(s=><option key={s}>{s}</option>)}
            </select>
          </div>
          <div>
            <label style={{fontSize:11.5,color:T.sub,fontWeight:600,display:"block",marginBottom:4}}>Min Locations</label>
            <input className="dl-input" type="number" min={1} value={minLoc} onChange={e=>setMinLoc(e.target.value)}/>
          </div>
          <button className="dl-btn" onClick={runSearch} disabled={loading} style={{background:`linear-gradient(135deg,${T.accent},${T.accent2})`,color:"#04121a",padding:"11px 22px",borderRadius:10,fontSize:14,height:42,boxShadow:`0 8px 20px -8px ${T.accent}`}}>
            {loading?<Loader2 size={17} style={{animation:"spin .7s linear infinite"}}/>:<Search size={16}/>}
            {loading?"Searching…":"Discover"}
          </button>
        </div>
      </div>

      {/* disclaimer */}
      <div style={{display:"flex",gap:9,alignItems:"flex-start",padding:"11px 14px",borderRadius:11,background:T.inputBg,border:`1px solid ${T.border}`,marginBottom:18,fontSize:12,color:T.sub,lineHeight:1.5}}>
        <Info size={15} color={T.accent} style={{flexShrink:0,marginTop:1}}/>
        <span>Results are compiled from public business information. In this demo they're generated locally to illustrate the workflow — connect a directory/social API in production. Always confirm contact details and respect opt-out / anti-spam rules before outreach.</span>
      </div>

      {/* loading skeleton */}
      {loading && (
        <div style={{display:"grid",gridTemplateColumns:"repeat(auto-fill,minmax(330px,1fr))",gap:14}}>
          {[0,1,2,3,4,5].map(i=>(
            <div key={i} className="dl-card" style={{padding:18,opacity:.5}}>
              <div style={{height:14,width:"60%",background:T.border,borderRadius:6,marginBottom:10}}/>
              <div style={{height:10,width:"40%",background:T.border,borderRadius:6,marginBottom:18}}/>
              <div style={{height:10,width:"90%",background:T.border,borderRadius:6,marginBottom:8}}/>
              <div style={{height:10,width:"75%",background:T.border,borderRadius:6}}/>
            </div>
          ))}
        </div>
      )}

      {/* results */}
      {results && !loading && (
        <div>
          <div style={{display:"flex",alignItems:"center",gap:12,marginBottom:14,flexWrap:"wrap"}}>
            <div style={{fontSize:14,fontWeight:700}}>{results.length} practices found</div>
            <span className="dl-chip" style={{background:T.accent2+"22",color:T.accent2,display:"flex",alignItems:"center",gap:5}}><Layers size={12}/>{multiCount} multi-location</span>
            <span className="dl-chip" style={{background:T.accent+"22",color:T.accent}}>{newCount} new to CRM</span>
            <button className="dl-btn" onClick={saveAll} disabled={newCount===0} style={{marginLeft:"auto",background:newCount?`linear-gradient(135deg,${T.accent},${T.accent2})`:T.inputBg,color:newCount?"#04121a":T.sub,padding:"9px 16px",borderRadius:10,fontSize:13,opacity:newCount?1:.6}}>
              <PlusCircle size={15}/>Save all new ({newCount})
            </button>
          </div>

          <div style={{display:"grid",gridTemplateColumns:"repeat(auto-fill,minmax(330px,1fr))",gap:14}}>
            {results.map(p=>{
              const known = isKnown(p);
              const state2 = saved[p._discoveryId];
              const done = state2==="added";
              const dup = state2==="duplicate" || known;
              return (
                <div key={p._discoveryId} className="dl-card dl-kpi" style={{padding:"16px 17px",display:"flex",flexDirection:"column",gap:11,borderTop:`3px solid ${scoreColor(p.score)}`}}>
                  <div style={{display:"flex",justifyContent:"space-between",gap:10}}>
                    <div>
                      <div style={{fontWeight:800,fontSize:14.5,display:"flex",alignItems:"center",gap:7,flexWrap:"wrap"}}>
                        {p.practiceName}
                        {p.locations>1 && <span className="dl-chip" style={{background:T.accent2+"22",color:T.accent2,display:"inline-flex",alignItems:"center",gap:3}}><Layers size={10}/>{p.locations} locations</span>}
                      </div>
                      <div style={{fontSize:12,color:T.sub,marginTop:2}}>{p.specialty} · {p.city}, {p.state}</div>
                    </div>
                    <div style={{textAlign:"center",flexShrink:0}}>
                      <div style={{width:42,height:42,borderRadius:11,background:scoreColor(p.score)+"22",display:"grid",placeItems:"center"}}>
                        <span style={{fontFamily:"Sora",fontWeight:800,fontSize:16,color:scoreColor(p.score)}}>{p.score}</span>
                      </div>
                      <div style={{fontSize:9.5,color:T.sub,marginTop:2,fontWeight:600}}>SCORE</div>
                    </div>
                  </div>

                  <div style={{display:"flex",flexDirection:"column",gap:5,fontSize:12.5}}>
                    <span style={{display:"flex",alignItems:"center",gap:7,color:T.sub}}><Building2 size={13}/>{p.contactName} · {p.dentists} dentist{p.dentists>1?"s":""}</span>
                    {p.email && <span style={{display:"flex",alignItems:"center",gap:7,color:T.sub}}><Mail size={13}/>{p.email}</span>}
                    {p.phone && <span style={{display:"flex",alignItems:"center",gap:7,color:T.sub}}><Phone size={13}/>{p.phone}</span>}
                  </div>

                  {/* extracted links */}
                  <div style={{display:"flex",alignItems:"center",gap:13,paddingTop:3}}>
                    <a href={p.website} target="_blank" rel="noreferrer" title="Website" style={{color:T.accent}}><Globe size={16}/></a>
                    {p.facebook && <a href={p.facebook} target="_blank" rel="noreferrer" title="Facebook" style={{color:"#1877f2"}}><Facebook size={16}/></a>}
                    {p.instagram && <a href={p.instagram} target="_blank" rel="noreferrer" title="Instagram" style={{color:"#e1306c"}}><Instagram size={16}/></a>}
                    {p.linkedin && <a href={p.linkedin} target="_blank" rel="noreferrer" title="LinkedIn" style={{color:"#0a66c2"}}><Linkedin size={16}/></a>}
                    <button className="dl-btn" onClick={()=>setOpenMsg(openMsg===p._discoveryId?null:p._discoveryId)} style={{marginLeft:"auto",background:"transparent",color:T.accent,fontSize:12,padding:0}}>
                      <Wand2 size={14}/>Outreach
                    </button>
                  </div>

                  {openMsg===p._discoveryId && (
                    <div className="dl-fade" style={{background:T.inputBg,borderRadius:10,padding:"11px 13px",fontSize:12.3,lineHeight:1.55,color:T.text,position:"relative"}}>
                      {p.outreach}
                      <button className="dl-btn" onClick={()=>{navigator.clipboard?.writeText(p.outreach);}} style={{marginTop:8,background:T.surface,color:T.sub,padding:"5px 10px",borderRadius:7,fontSize:11,border:`1px solid ${T.border}`}}><Copy size={12}/>Copy message</button>
                    </div>
                  )}

                  <button className="dl-btn" disabled={done||dup} onClick={()=>saveOne(p)} style={{
                    marginTop:"auto",justifyContent:"center",padding:"9px 0",borderRadius:10,fontSize:13,
                    background: done?T.accent2+"22" : dup?T.inputBg : `linear-gradient(135deg,${T.accent},${T.accent2})`,
                    color: done?T.accent2 : dup?T.sub : "#04121a",
                    border: dup&&!done?`1px solid ${T.border}`:"none", cursor:(done||dup)?"default":"pointer"}}>
                    {done ? <><CheckCircle2 size={15}/>Saved to CRM</> : dup ? <><AlertTriangle size={14}/>Already in CRM</> : <><Plus size={15}/>Save to CRM</>}
                  </button>
                </div>
              );
            })}
          </div>
        </div>
      )}

      {/* empty state */}
      {!results && !loading && (
        <div className="dl-card" style={{padding:"54px 20px",textAlign:"center"}}>
          <div style={{width:60,height:60,borderRadius:16,background:T.accent+"18",display:"grid",placeItems:"center",margin:"0 auto 16px"}}><Radar size={28} color={T.accent}/></div>
          <div style={{fontWeight:700,fontSize:15}}>Start discovering leads</div>
          <div style={{fontSize:13,color:T.sub,marginTop:6,maxWidth:380,margin:"6px auto 0"}}>Pick a state and specialty above, then run a search to surface dental practices with extracted contact details, social profiles, and auto-assigned lead scores.</div>
        </div>
      )}
    </div>
  );
}
