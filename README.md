
### Feature Selection

"Make everything as simple as possible, but no simpler."  -- Albert Einstein


```Python

# A new Enron feature quiz
# The following codes can only run in Udacity quiz.
#!/usr/bin/python

### in poiFlagEmail() below, write code that returns a boolean
### indicating if a given email is from a POI

import sys
import reader #only available on Udacity
import poi_emails

def getToFromStrings(f):
    '''
    The imported reader.py file contains functions that we've created to help
    parse e-mails from the corpus. .getAddresses() reads in the opening lines
    of an e-mail to find the To: From: and CC: strings, while the
    .parseAddresses() line takes each string and extracts the e-mail addresses
    as a list.
    '''
    f.seek(0)
    to_string, from_string, cc_string   = reader.getAddresses(f)
    to_emails   = reader.parseAddresses( to_string )
    from_emails = reader.parseAddresses( from_string )
    cc_emails   = reader.parseAddresses( cc_string )

    return to_emails, from_emails, cc_emails

### POI flag an email

def poiFlagEmail(f):
    """ given an email file f,
        return a trio of booleans for whether that email is
        to, from, or cc'ing a poi """

    to_emails, from_emails, cc_emails = getToFromStrings(f)

    ### poi_emails.poiEmails() returns a list of all POIs' email addresses.
    poi_email_list = poi_emails.poiEmails()

    to_poi = False
    from_poi = False
    cc_poi   = False

    ### to_poi and cc_poi are boolean variables which flag whether the email
    ### under inspection is addressed to a POI, or if a POI is in cc,
    ### respectively. You don't have to change this code at all.

    ### There can be many "to" emails, but only one "from", so the
    ### "to" processing needs to be a little more complicated
    if to_emails:
        ctr = 0
        while not to_poi and ctr < len(to_emails):
            if to_emails[ctr] in poi_email_list:
                to_poi = True
            ctr += 1
    if cc_emails:
        ctr = 0
        while not cc_poi and ctr < len(cc_emails):
            if cc_emails[ctr] in poi_email_list:
                cc_poi = True
            ctr += 1

    #################################
    ######## your code below ########
    ### set from_poi to True if #####
    ### the email is from a POI #####
    #################################

    if from_emails:
        ctr = 0
        while not from_poi and ctr < len(from_emails):
            if from_emails[ctr] in poi_email_list:
                from_poi = True
            ctr += 1

    #################################
    return to_poi, from_poi, cc_poi

```

```Python
#!/usr/bin/python

import os
import sys
import zipfile
from poi_flag_email import poiFlagEmail, getToFromStrings

data_dict = {}

with zipfile.ZipFile('emails.zip', "r") as z:
    z.extractall()

for email_message in os.listdir("emails"):
    if email_message == ".DS_Store":
        continue
    message = open(os.getcwd()+"/emails/"+email_message, "r")
    to_addresses, from_addresses, cc_addresses = getToFromStrings(message)

    to_poi, from_poi, cc_poi = poiFlagEmail(message)

    for recipient in to_addresses:
        # initialize counter
        if recipient not in data_dict:
            data_dict[recipient] = {"from_poi_to_this_person":0}
        # add to count
        if from_poi:
                data_dict[recipient]["from_poi_to_this_person"] += 1

    message.close()

for item in data_dict:
    print item, data_dict[item]

#######################################################    
def submitData():
    return data_dict
```

Here's your output:

{'ken.rice@enron.com': {'from_poi_to_this_person': 0}, 'sheila.knudsen@enron.com': {'from_poi_to_this_person': 1}, 'mark.bernstein@enron.com': {'from_poi_to_this_person': 0}, 'david.w.delainey@enron.com': {'from_poi_to_this_person': 0}, 'mark.koenig@enron.com': {'from_poi_to_this_person': 0}, 'suzanne.danz@enron.com': {'from_poi_to_this_person': 0}, 'paul.clayton@enron.com': {'from_poi_to_this_person': 1}, 'loretta.brelsford@enron.com': {'from_poi_to_this_person': 0}, 'michelle.parks@enron.com': {'from_poi_to_this_person': 1}, 'jeffrey.shankman@enron.com': {'from_poi_to_this_person': 11}, 'ryan.watt@enron.com': {'from_poi_to_this_person': 0}, 'antonio.kuehl@enron.com': {'from_poi_to_this_person': 0}, 'jana.paxton@enron.com': {'from_poi_to_this_person': 0}, 'craig.taylor@enron.com': {'from_poi_to_this_person': 0}, 'michael.bilberry@enron.com': {'from_poi_to_this_person': 0}, 'david.delainey@enron.com': {'from_poi_to_this_person': 1}, 'james.derrick@enron.com': {'from_poi_to_this_person': 0}, 'john.normand@enron.com': {'from_poi_to_this_person': 1}, 'nicole.la@enron.com': {'from_poi_to_this_person': 0}, 'gautam.gupta@enron.com': {'from_poi_to_this_person': 0}, 'tori.wells@enron.com': {'from_poi_to_this_person': 0}, 'kate.chaney@enron.com': {'from_poi_to_this_person': 0}, 'rogers.herndon@enron.com': {'from_poi_to_this_person': 1}, 'randall.curry@enron.com': {'from_poi_to_this_person': 2}, 'stuart.zisman@enron.com': {'from_poi_to_this_person': 1}, 'vanessa.groscrand@enron.com': {'from_poi_to_this_person': 0}, 'bryan.hull@enron.com': {'from_poi_to_this_person': 0}, 'kevin.mcgowan@enron.com': {'from_poi_to_this_person': 2}, 'steve.elliott@enron.com': {'from_poi_to_this_person': 0}, 'wdavid.duran@enron.com': {'from_poi_to_this_person': 0}, 'louise.kitchen@enron.com': {'from_poi_to_this_person': 0}, 'cliff.baxter@enron.com': {'from_poi_to_this_person': 0}, 'tom.swank@enron.com': {'from_poi_to_this_person': 1}, 'joseph@enron.com': {'from_poi_to_this_person': 0}, 'greg.piper@enron.com': {'from_poi_to_this_person': 2}, 'vladi.pimenov@enron.com': {'from_poi_to_this_person': 0}, 'daniel.reck@enron.com': {'from_poi_to_this_person': 2}, 'randal.maffett@enron.com': {'from_poi_to_this_person': 3}, 'yvette.parker@enron.com': {'from_poi_to_this_person': 0}, 'fred.kelly@enron.com': {'from_poi_to_this_person': 3}, 'howard.fromer@enron.com': {'from_poi_to_this_person': 0}, 'james.coffey@enron.com': {'from_poi_to_this_person': 0}, 'kevin.hannon@enron.com': {'from_poi_to_this_person': 0}, 'david.cox@enron.com': {'from_poi_to_this_person': 0}, 'david.marshall@enron.com': {'from_poi_to_this_person': 1}, 'Follow-uptoCommentsinensideMime-Version': {'from_poi_to_this_person': 1}, 'john.arnold@enron.com': {'from_poi_to_this_person': 5}, 'george.wood@enron.com': {'from_poi_to_this_person': 0}, 'timothy.detmering@enron.com': {'from_poi_to_this_person': 1}, 'QBRPrincipalInvestments;JeffDonahue': {'from_poi_to_this_person': 0}, 'james.ducote@enron.com': {'from_poi_to_this_person': 0}, 'ben.jacoby@enron.com': {'from_poi_to_this_person': 9}, 'william.keeney@enron.com': {'from_poi_to_this_person': 0}, 'victor.guggenheim@enron.com': {'from_poi_to_this_person': 0}, 'chetan.paipanandiker@enron.com': {'from_poi_to_this_person': 0}, 'splauch@enron.com': {'from_poi_to_this_person': 0}, 'greg.whiting@enron.com': {'from_poi_to_this_person': 0}, 'fred.lagrasta@enron.com': {'from_poi_to_this_person': 1}, 'darren.espey@enron.com': {'from_poi_to_this_person': 0}, 'christopher.calger@enron.com': {'from_poi_to_this_person': 30}, 'joe.kishkill@enron.com': {'from_poi_to_this_person': 5}, 'john.king@enron.com': {'from_poi_to_this_person': 0}, 'ashley.worthing@enron.com': {'from_poi_to_this_person': 0}, 'evan.betzer@enron.com': {'from_poi_to_this_person': 0}, 'jennifer.riley@enron.com': {'from_poi_to_this_person': 0}, 'judy.smith@enron.com': {'from_poi_to_this_person': 0}, 'michael.galvan@enron.com': {'from_poi_to_this_person': 0}, 'todd.busby@enron.com': {'from_poi_to_this_person': 1}, 'michelle.zhang@enron.com': {'from_poi_to_this_person': 0}, 'kathy.campos@enron.com': {'from_poi_to_this_person': 0}, 'McConnellandShankman3321(Freverthandling)Mime-Version': {'from_poi_to_this_person': 0}, 'terry.donovan@enron.com': {'from_poi_to_this_person': 1}, 'kimat.singla@enron.com': {'from_poi_to_this_person': 0}, 'david.guillaume@enron.com': {'from_poi_to_this_person': 0}, 'john.disturnal@enron.com': {'from_poi_to_this_person': 0}, 'barbara.gray@enron.com': {'from_poi_to_this_person': 0}, 'maureen.mcvicker@enron.com': {'from_poi_to_this_person': 0}, 'cooper.richey@enron.com': {'from_poi_to_this_person': 0}, 'jonathan.hoff@enron.com': {'from_poi_to_this_person': 0}, 'marc.sabine@enron.com': {'from_poi_to_this_person': 0}, 'jennifer.burns@enron.com': {'from_poi_to_this_person': 0}, 'jordan.mintz@enron.com': {'from_poi_to_this_person': 2}, 'michael.bradley@enron.com': {'from_poi_to_this_person': 0}, 'brian.hoskins@enron.com': {'from_poi_to_this_person': 0}, 'greg.hermans@enron.com': {'from_poi_to_this_person': 0}, 'paul.gregory@enron.com': {'from_poi_to_this_person': 0}, 'ed.mcmichael@enron.com': {'from_poi_to_this_person': 0}, 'robert.stalford@enron.com': {'from_poi_to_this_person': 0}, 'douglas.dunn@enron.com': {'from_poi_to_this_person': 0}, 'kulvinder.fowler@enron.com': {'from_poi_to_this_person': 0}, 'tchurbuck@powermfg.com': {'from_poi_to_this_person': 1}, 'jake.thomas@enron.com': {'from_poi_to_this_person': 0}, 'andrew.fastow@enron.com': {'from_poi_to_this_person': 1}, 'shona.wilson@enron.com': {'from_poi_to_this_person': 0}, 'fletcher.sturm@enron.com': {'from_poi_to_this_person': 1}, 'krista.kisch@enron.com': {'from_poi_to_this_person': 1}, 'melissa.jones@enron.com': {'from_poi_to_this_person': 0}, 'lauren.urquhart@enron.com': {'from_poi_to_this_person': 0}, 'colleen.sullivan@enron.com': {'from_poi_to_this_person': 1}, 'celeste.roberts@enron.com': {'from_poi_to_this_person': 2}, 'john.zufferli@enron.com': {'from_poi_to_this_person': 1}, 'posey.martinez@enron.com': {'from_poi_to_this_person': 0}, 'john.thompson@enron.com': {'from_poi_to_this_person': 1}, 'benjamin.rogers@enron.com': {'from_poi_to_this_person': 1}, 'homer.lin@enron.com': {'from_poi_to_this_person': 0}, 'stephanie.harris@enron.com': {'from_poi_to_this_person': 0}, 'chris.lambie@enron.com': {'from_poi_to_this_person': 0}, 'airam.arteaga@enron.com': {'from_poi_to_this_person': 0}, 'ross.newlin@enron.com': {'from_poi_to_this_person': 0}, 'victoria.versen@enron.com': {'from_poi_to_this_person': 0}, 'NickCocavessisre': {'from_poi_to_this_person': 0}, 'patti.thompson@enron.com': {'from_poi_to_this_person': 0}, 'lloyd.will@enron.com': {'from_poi_to_this_person': 0}, 'grant.masson@enron.com': {'from_poi_to_this_person': 0}, 'chris.booth@enron.com': {'from_poi_to_this_person': 0}, 'susan.scott@enron.com': {'from_poi_to_this_person': 0}, 'mark.palmer@enron.com': {'from_poi_to_this_person': 2}, 'shawn.anderson@enron.com': {'from_poi_to_this_person': 0}, 'derek.lynn@enron.com': {'from_poi_to_this_person': 0}, 'tyrell.harrison@enron.com': {'from_poi_to_this_person': 0}, 'cyntia.distefano@enron.com': {'from_poi_to_this_person': 0}, 'michael.miller@enron.com': {'from_poi_to_this_person': 5}, 'chris.dorland@enron.com': {'from_poi_to_this_person': 0}, 'jesus.melendrez@enron.com': {'from_poi_to_this_person': 0}, 'laura.luce@enron.com': {'from_poi_to_this_person': 9}, 'hope.vargas@enron.com': {'from_poi_to_this_person': 0}, 'lisa.zarsky@enron.com': {'from_poi_to_this_person': 0}, 'catherine.clark@enron.com': {'from_poi_to_this_person': 0}, 'george.mcclellan@enron.com': {'from_poi_to_this_person': 5}, 'ranabir.dutt@enron.com': {'from_poi_to_this_person': 0}, 'adnan.patel@enron.com': {'from_poi_to_this_person': 1}, 'jason.seigal@enron.com': {'from_poi_to_this_person': 0}, 'william.bradford@enron.com': {'from_poi_to_this_person': 2}, 'OpposingPUHCAStand-AloneMime-Version': {'from_poi_to_this_person': 1}, 'patrick.wade@enron.com': {'from_poi_to_this_person': 1}, 'howard.sangwine@enron.com': {'from_poi_to_this_person': 0}, 'michael.beyer@enron.com': {'from_poi_to_this_person': 2}, 'jay.epstein@enron.com': {'from_poi_to_this_person': 0}, 'connie.blackwood@enron.com': {'from_poi_to_this_person': 0}, 'paul.quilkey@enron.com': {'from_poi_to_this_person': 2}, 'william.rome@enron.com': {'from_poi_to_this_person': 0}, 'vasant.shanbhogue@enron.com': {'from_poi_to_this_person': 0}, 'shawn.cumberland@enron.com': {'from_poi_to_this_person': 4}, 'kevin.communications@enron.com': {'from_poi_to_this_person': 0}, 'james.hungerford@enron.com': {'from_poi_to_this_person': 0}, 'jonathon.pielop@enron.com': {'from_poi_to_this_person': 0}, 'kay.chapman@enron.com': {'from_poi_to_this_person': 66}, 'tim.belden@enron.com': {'from_poi_to_this_person': 19}, 'carol.brown@enron.com': {'from_poi_to_this_person': 0}, 'mike.jakubik@enron.com': {'from_poi_to_this_person': 0}, 'paul.devries@enron.com': {'from_poi_to_this_person': 3}, 'richard.causey@enron.com': {'from_poi_to_this_person': 1}, 'barry.tycholiz@enron.com': {'from_poi_to_this_person': 7}, 'berney.aucoin@enron.com': {'from_poi_to_this_person': 0}, 'kyle.etter@enron.com': {'from_poi_to_this_person': 0}, 'david.haug@enron.com': {'from_poi_to_this_person': 1}, 'clement.lau@enron.com': {'from_poi_to_this_person': 0}, 'jere.overdyke@enron.com': {'from_poi_to_this_person': 4}, 'jeff.skilling@enron.com': {'from_poi_to_this_person': 3}, 'ibrahim.qureishi@enron.com': {'from_poi_to_this_person': 0}, 'beverly.stephens@enron.com': {'from_poi_to_this_person': 0}, 'Re': {'from_poi_to_this_person': 1}, 'john.massey@enron.com': {'from_poi_to_this_person': 0}, 'lea.savala@enron.com': {'from_poi_to_this_person': 0}, 'charles.varnell@enron.com': {'from_poi_to_this_person': 0}, 'marion.cole@enron.com': {'from_poi_to_this_person': 1}, 'scott.healy@enron.com': {'from_poi_to_this_person': 0}, 'cathy.phillips@enron.com': {'from_poi_to_this_person': 0}, 'remi.collonges@enron.com': {'from_poi_to_this_person': 0}, 'david.communications@enron.com': {'from_poi_to_this_person': 0}, 'jader@enron.com': {'from_poi_to_this_person': 0}, 'travis.winfrey@enron.com': {'from_poi_to_this_person': 0}, 'ken.communications@enron.com': {'from_poi_to_this_person': 0}, 'edie.leschber@enron.com': {'from_poi_to_this_person': 0}, 'gaurav.babbar@enron.com': {'from_poi_to_this_person': 0}, 'drew.tingleaf@enron.com': {'from_poi_to_this_person': 0}, 'scott.neal@enron.com': {'from_poi_to_this_person': 6}, 'adam.umanoff@enron.com': {'from_poi_to_this_person': 2}, 'kathy.mcmahon@enron.com': {'from_poi_to_this_person': 0}, 'enron.administration@enron.com': {'from_poi_to_this_person': 0}, 'greg.martin@enron.com': {'from_poi_to_this_person': 0}, "tim.o'rourke@enron.com": {'from_poi_to_this_person': 1}, 'julie.gomez@enron.com': {'from_poi_to_this_person': 2}, 'rob.milnthorp@enron.com': {'from_poi_to_this_person': 37}, 'lisa.norman@enron.com': {'from_poi_to_this_person': 0}, 'stanton.ray@enron.com': {'from_poi_to_this_person': 0}, 'brett.wiggs@enron.com': {'from_poi_to_this_person': 2}, 'daniel.allegretti@enron.com': {'from_poi_to_this_person': 0}, 'thomas.a.martin@enron.com': {'from_poi_to_this_person': 0}, 'stanley.horton@enron.com': {'from_poi_to_this_person': 0}, 'liz.taylor@enron.com': {'from_poi_to_this_person': 0}, 'jim.fallon@enron.com': {'from_poi_to_this_person': 0}, 'robin.rodrigue@enron.com': {'from_poi_to_this_person': 0}, 'c.thompson@enron.com': {'from_poi_to_this_person': 12}, 'janice.priddy@enron.com': {'from_poi_to_this_person': 0}, 'Hodge': {'from_poi_to_this_person': 0}, 'peter.keohane@enron.com': {'from_poi_to_this_person': 1}, 'don.miller@enron.com': {'from_poi_to_this_person': 11}, 'grant.oh@enron.com': {'from_poi_to_this_person': 0}, 'john.sherriff@enron.com': {'from_poi_to_this_person': 0}, 'jeffrey.keenan@enron.com': {'from_poi_to_this_person': 0}, 'sue.ford@enron.com': {'from_poi_to_this_person': 0}, 'tom.dutta@enron.com': {'from_poi_to_this_person': 0}, 'donna.baker@enron.com': {'from_poi_to_this_person': 0}, 'mike.swerzbin@enron.com': {'from_poi_to_this_person': 1}, 'brian.redmond@enron.com': {'from_poi_to_this_person': 27}, 'chip.schneider@enron.com': {'from_poi_to_this_person': 1}, 'lon.draper@enron.com': {'from_poi_to_this_person': 0}, 'kenneth.lay@enron.com': {'from_poi_to_this_person': 4}, 'gregg.penman@enron.com': {'from_poi_to_this_person': 2}, 'elizabeth.sager@enron.com': {'from_poi_to_this_person': 1}, 'gabriel.monroy@enron.com': {'from_poi_to_this_person': 0}, 'stephen.thatcher@enron.com': {'from_poi_to_this_person': 0}, 'steve.montovano@enron.com': {'from_poi_to_this_person': 1}, 'jim.schwieger@enron.com': {'from_poi_to_this_person': 0}, 'yvan.chaxel@enron.com': {'from_poi_to_this_person': 0}, 'sherri.sera@enron.com': {'from_poi_to_this_person': 0}, 'thomas.suffield@enron.com': {'from_poi_to_this_person': 0}, 'lance.schuler-legal@enron.com': {'from_poi_to_this_person': 0}, 'tammy.shepperd@enron.com': {'from_poi_to_this_person': 6}, 'charles.ward@enron.com': {'from_poi_to_this_person': 1}, 'BobVirgo': {'from_poi_to_this_person': 0}, 'chris.h.foster@enron.com': {'from_poi_to_this_person': 0}, 'scott.goodell@enron.com': {'from_poi_to_this_person': 0}, 'monica.reasoner@enron.com': {'from_poi_to_this_person': 0}, 'narsimha.misra@enron.com': {'from_poi_to_this_person': 0}, 'diomedes.christodoulou@enron.com': {'from_poi_to_this_person': 0}, 'christina.grow@enron.com': {'from_poi_to_this_person': 0}, 'pallen@enron.com': {'from_poi_to_this_person': 0}, 'hardie.davis@enron.com': {'from_poi_to_this_person': 0}, 'alejandra.chavez@enron.com': {'from_poi_to_this_person': 0}, 'MeetingwithWayneMays': {'from_poi_to_this_person': 0}, 'mary.perkins@enron.com': {'from_poi_to_this_person': 0}, 'sanjay.bhatnagar@enron.com': {'from_poi_to_this_person': 0}, 'dawn.doucet@enron.com': {'from_poi_to_this_person': 1}, 'kam.keiser@enron.com': {'from_poi_to_this_person': 0}, 'milind.pasad@enron.com': {'from_poi_to_this_person': 0}, 'leah.rijo@enron.com': {'from_poi_to_this_person': 0}, 'jessica.ramirez@enron.com': {'from_poi_to_this_person': 0}, 'dlhart1@aep.com': {'from_poi_to_this_person': 0}, 'jason.wiesepape@enron.com': {'from_poi_to_this_person': 0}, 'frank.w.vickers@enron.com': {'from_poi_to_this_person': 0}, 'jeff.davis@enron.com': {'from_poi_to_this_person': 1}, 'mitch.robinson@enron.com': {'from_poi_to_this_person': 2}, 'marty.sunde@enron.com': {'from_poi_to_this_person': 4}, 'f..brawner@enron.com': {'from_poi_to_this_person': 0}, 'c.john.thompson@enron.com': {'from_poi_to_this_person': 0}, 'wes.colwell@enron.com': {'from_poi_to_this_person': 28}, 'mary.mckendree@enron.com': {'from_poi_to_this_person': 2}, 'susan.helton@enron.com': {'from_poi_to_this_person': 1}, 'brian.bierbach@enron.com': {'from_poi_to_this_person': 2}, 'max.yzaguirre@enron.com': {'from_poi_to_this_person': 13}, 'phillip.allen@enron.com': {'from_poi_to_this_person': 6}, 'chad.clark@enron.com': {'from_poi_to_this_person': 0}, 'kerry.ferrari@enron.com': {'from_poi_to_this_person': 0}, 'sarah.mulholland@enron.com': {'from_poi_to_this_person': 0}, 'wayne.mays@enron.com': {'from_poi_to_this_person': 5}, 'susan.skarness@enron.com': {'from_poi_to_this_person': 0}, 'jason.biever@enron.com': {'from_poi_to_this_person': 0}, 'matthew.lenhart@enron.com': {'from_poi_to_this_person': 0}, 'sap.3@enron.com': {'from_poi_to_this_person': 0}, 'shari.mao@enron.com': {'from_poi_to_this_person': 0}, 'brad.mckay@enron.com': {'from_poi_to_this_person': 0}, 'edward.ondarza@enron.com': {'from_poi_to_this_person': 1}, 'michael.moulton@enron.com': {'from_poi_to_this_person': 1}, 'jennifer.adams@enron.com': {'from_poi_to_this_person': 0}, 'mark.metts@enron.com': {'from_poi_to_this_person': 0}, 'derek.davies@enron.com': {'from_poi_to_this_person': 1}, 'mathew.gimble@enron.com': {'from_poi_to_this_person': 1}, 'fiona.stewart@enron.com': {'from_poi_to_this_person': 0}, 'jeffrey.sherrick@enron.com': {'from_poi_to_this_person': 4}, 'joe.gordon@enron.com': {'from_poi_to_this_person': 0}, 'mark.tawney@enron.com': {'from_poi_to_this_person': 0}, 'doug.paterson@enron.com': {'from_poi_to_this_person': 0}, 'stephen.wolfe@enron.com': {'from_poi_to_this_person': 0}, 'benjamin.thomason@enron.com': {'from_poi_to_this_person': 1}, 'christopher.helfrich@enron.com': {'from_poi_to_this_person': 0}, 'david.gorte@enron.com': {'from_poi_to_this_person': 5}, 'cliff.shedd@enron.com': {'from_poi_to_this_person': 0}, 'cindy.stark@enron.com': {'from_poi_to_this_person': 0}, 'steven.schneider@enron.com': {'from_poi_to_this_person': 0}, 'mark.taylor@enron.com': {'from_poi_to_this_person': 0}, 'jeff.royed@enron.com': {'from_poi_to_this_person': 0}, 'douglas.clifford@enron.com': {'from_poi_to_this_person': 0}, 'jinsung.myung@enron.com': {'from_poi_to_this_person': 0}, 'jeremy.blachman@enron.com': {'from_poi_to_this_person': 0}, 'mercedes.estrada@enron.com': {'from_poi_to_this_person': 0}, 'ojzeringue@tva.gov': {'from_poi_to_this_person': 4}, 'charles.vetters@enron.com': {'from_poi_to_this_person': 4}, 'kevin.presto@enron.com': {'from_poi_to_this_person': 23}, 'erin.rice@enron.com': {'from_poi_to_this_person': 1}, 'katherine.brown@enron.com': {'from_poi_to_this_person': 0}, 'christi.nicolay@enron.com': {'from_poi_to_this_person': 5}, 'mark.e.haedicke@enron.com': {'from_poi_to_this_person': 0}, 'deb.korkmas@enron.com': {'from_poi_to_this_person': 0}, 'jeff.pearson@enron.com': {'from_poi_to_this_person': 0}, 'orlando.gonzalez@enron.com': {'from_poi_to_this_person': 3}, 'melissa.videtto@enron.com': {'from_poi_to_this_person': 0}, 'lou.pai@enron.com': {'from_poi_to_this_person': 1}, 'jesse.neyman@enron.com': {'from_poi_to_this_person': 0}, 'john.swafford@enron.com': {'from_poi_to_this_person': 0}, 'mark.dobler@enron.com': {'from_poi_to_this_person': 7}, 'alan.aronowitz@enron.com': {'from_poi_to_this_person': 0}, 'danny.mccarty@enron.com': {'from_poi_to_this_person': 0}, 'dana.davis@enron.com': {'from_poi_to_this_person': 1}, 'david.oxley@enron.com': {'from_poi_to_this_person': 31}, 'claudio.ribeiro@enron.com': {'from_poi_to_this_person': 0}, 'andrea.ring@enron.com': {'from_poi_to_this_person': 0}, 'john.howton@enron.com': {'from_poi_to_this_person': 0}, 'maribel.mata@enron.com': {'from_poi_to_this_person': 0}, 'Reck': {'from_poi_to_this_person': 0}, 'crystal.hyde@enron.com': {'from_poi_to_this_person': 0}, 'suzanne.nicholie@enron.com': {'from_poi_to_this_person': 0}, 'donald.black@enron.com': {'from_poi_to_this_person': 0}, 'kathy.dodgen@enron.com': {'from_poi_to_this_person': 0}, 'jana.giovannini@enron.com': {'from_poi_to_this_person': 2}, 'lisa.gillette@enron.com': {'from_poi_to_this_person': 0}, 'robert.hermann@enron.com': {'from_poi_to_this_person': 1}, 'vince.kaminski@enron.com': {'from_poi_to_this_person': 3}, 'rick.whitaker@enron.com': {'from_poi_to_this_person': 1}, 'hunter.s.shively@enron.com': {'from_poi_to_this_person': 0}, 'rebecca.mcdonald@enron.com': {'from_poi_to_this_person': 5}, "d'arcy.carroll@enron.com": {'from_poi_to_this_person': 1}, 'brenda.castillo@enron.com': {'from_poi_to_this_person': 0}, 'McGowan': {'from_poi_to_this_person': 0}, 'james.hughes@enron.com': {'from_poi_to_this_person': 4}, 'kathryn.greer@enron.com': {'from_poi_to_this_person': 0}, 'eric.gadd@enron.com': {'from_poi_to_this_person': 1}, 'rod.hayslett@enron.com': {'from_poi_to_this_person': 0}, 'tom.may@enron.com': {'from_poi_to_this_person': 0}, 'eric.bass@enron.com': {'from_poi_to_this_person': 0}, 'wanda.labaume@enron.com': {'from_poi_to_this_person': 0}, 'stephane.brodeur@enron.com': {'from_poi_to_this_person': 0}, 'eric.thode@enron.com': {'from_poi_to_this_person': 9}, 'ted.bland@enron.com': {'from_poi_to_this_person': 2}, 'farzad.farhangnia@enron.com': {'from_poi_to_this_person': 0}, 'elaine.rodriguez@enron.com': {'from_poi_to_this_person': 0}, 'rick.carson@enron.com': {'from_poi_to_this_person': 1}, 'jeff.kinneman@enron.com': {'from_poi_to_this_person': 0}, 'rick.buy@enron.com': {'from_poi_to_this_person': 8}, 'andrea.reed@enron.com': {'from_poi_to_this_person': 2}, 'charlene.jackson@enron.com': {'from_poi_to_this_person': 3}, 'marcia.manarin@enron.com': {'from_poi_to_this_person': 0}, 'peggy.hedstrom@enron.com': {'from_poi_to_this_person': 0}, 'elizabeth.mccarthy@enron.com': {'from_poi_to_this_person': 0}, 'nick.hiemstra@enron.com': {'from_poi_to_this_person': 0}, 'philippe.bibi@enron.com': {'from_poi_to_this_person': 3}, 'scott.affelt@enron.com': {'from_poi_to_this_person': 2}, 'joseph.deffner@enron.com': {'from_poi_to_this_person': 12}, 'paul.adair@enron.com': {'from_poi_to_this_person': 0}, 'jeff.donahue@enron.com': {'from_poi_to_this_person': 22}, 'steve.pruett@enron.com': {'from_poi_to_this_person': 3}, 'ted.murphy@enron.com': {'from_poi_to_this_person': 1}, 'lavorato@enron.com': {'from_poi_to_this_person': 0}, 'susan.lewis@enron.com': {'from_poi_to_this_person': 0}, 'peter.bennett@enron.com': {'from_poi_to_this_person': 0}, "l'sheryl.hudson@enron.com": {'from_poi_to_this_person': 0}, 'carol.marshall@enron.com': {'from_poi_to_this_person': 1}, 'michael.guerriero@enron.com': {'from_poi_to_this_person': 2}, 'rosane.fabozzi@enron.com': {'from_poi_to_this_person': 0}, 'mike.miller@enron.com': {'from_poi_to_this_person': 11}, 'bryan.burnett@enron.com': {'from_poi_to_this_person': 1}, 'michelle.waldhauser@enron.com': {'from_poi_to_this_person': 0}, 'elizabeth.shim@enron.com': {'from_poi_to_this_person': 0}, 'scott.palmer@enron.com': {'from_poi_to_this_person': 0}, 'joseph.short@enron.com': {'from_poi_to_this_person': 2}, 'gregory.sharp@enron.com': {'from_poi_to_this_person': 0}, 'ozzie.pagan@enron.com': {'from_poi_to_this_person': 8}, 'cary.carrabine@enron.com': {'from_poi_to_this_person': 0}, 'MikeLMillerDelaineyEB3314Mime-Version': {'from_poi_to_this_person': 0}, 'bob.crane@enron.com': {'from_poi_to_this_person': 0}, 'michael.savidant@enron.com': {'from_poi_to_this_person': 0}, 'flagrasta@enron.com': {'from_poi_to_this_person': 0}, 'kelly.johnson@enron.com': {'from_poi_to_this_person': 3}, 'gary.hickerson@enron.com': {'from_poi_to_this_person': 1}, 'billy.lemmons@enron.com': {'from_poi_to_this_person': 0}, 'doug.jones@enron.com': {'from_poi_to_this_person': 1}, 'david.howe@enron.com': {'from_poi_to_this_person': 0}, 'julie.armstrong@enron.com': {'from_poi_to_this_person': 0}, 'monica.butler@enron.com': {'from_poi_to_this_person': 0}, 'marsha.schiller@enron.com': {'from_poi_to_this_person': 0}, 'w.duran@enron.com': {'from_poi_to_this_person': 17}, 'donald.miller@enron.com': {'from_poi_to_this_person': 0}, 'karen.owens@enron.com': {'from_poi_to_this_person': 0}, 'kathryn.corbally@enron.com': {'from_poi_to_this_person': 1}, 'venita.coleman@enron.com': {'from_poi_to_this_person': 1}, 'eric.ledain@enron.com': {'from_poi_to_this_person': 8}, 'chris.foster@enron.com': {'from_poi_to_this_person': 5}, 'Delainey': {'from_poi_to_this_person': 0}, 'chris.gaskill@enron.com': {'from_poi_to_this_person': 0}, 'patti.ucci@enron.com': {'from_poi_to_this_person': 0}, 'chris.germany@enron.com': {'from_poi_to_this_person': 0}, 'jennifer.martinez@enron.com': {'from_poi_to_this_person': 1}, 'greg.blair@enron.com': {'from_poi_to_this_person': 1}, 'jeffery.ader@enron.com': {'from_poi_to_this_person': 6}, 'mike.coleman@enron.com': {'from_poi_to_this_person': 4}, 'mike.roberts@enron.com': {'from_poi_to_this_person': 0}, 'jaime.alatorre@enron.com': {'from_poi_to_this_person': 0}, 'rodney.malcolm@enron.com': {'from_poi_to_this_person': 12}, 'gil.muhl@enron.com': {'from_poi_to_this_person': 0}, 'doug.sewell@enron.com': {'from_poi_to_this_person': 0}, 'aleck.dadson@enron.com': {'from_poi_to_this_person': 0}, 'claire.broido@enron.com': {'from_poi_to_this_person': 0}, 'nick.cocavessis@enron.com': {'from_poi_to_this_person': 0}, 'f..keavey@enron.com': {'from_poi_to_this_person': 0}, 'lillian.carroll@enron.com': {'from_poi_to_this_person': 0}, 'michael.cowan@enron.com': {'from_poi_to_this_person': 0}, 'bruce.lundstrom@enron.com': {'from_poi_to_this_person': 1}, 'kevin.ruscitti@enron.com': {'from_poi_to_this_person': 0}, 'Cc': {'from_poi_to_this_person': 0}, 'raymond.bowen@enron.com': {'from_poi_to_this_person': 14}, 'all.worldwide@enron.com': {'from_poi_to_this_person': 0}, 'craig.fox@enron.com': {'from_poi_to_this_person': 0}, 'leonard.tham@enron.com': {'from_poi_to_this_person': 0}, 'rlee@waltonemc.com': {'from_poi_to_this_person': 1}, 'sheetal.patel@enron.com': {'from_poi_to_this_person': 0}, 'chad.gronvold@enron.com': {'from_poi_to_this_person': 0}, 'stuart.staley@enron.com': {'from_poi_to_this_person': 0}, 'mmiller3@enron.com': {'from_poi_to_this_person': 0}, 'steve.communications@enron.com': {'from_poi_to_this_person': 0}, 'hal.mckinney@enron.com': {'from_poi_to_this_person': 0}, 'bridget.maronge@enron.com': {'from_poi_to_this_person': 0}, 'peggy.mccurley@enron.com': {'from_poi_to_this_person': 0}, 'project.gem@enron.com': {'from_poi_to_this_person': 1}, 'mo.bawa@enron.com': {'from_poi_to_this_person': 0}, 'cindy.justice@enron.com': {'from_poi_to_this_person': 1}, 'bill.greenizan@enron.com': {'from_poi_to_this_person': 0}, 'janelle.scheuer@enron.com': {'from_poi_to_this_person': 1}, 'john.llodra@enron.com': {'from_poi_to_this_person': 1}, 'sally.beck@enron.com': {'from_poi_to_this_person': 10}, 'johnson.leo@enron.com': {'from_poi_to_this_person': 0}, 'stacy.guidroz@enron.com': {'from_poi_to_this_person': 0}, 'stephen.douglas@enron.com': {'from_poi_to_this_person': 6}, 'brent.tiner@enron.com': {'from_poi_to_this_person': 1}, 'stinson.gibner@enron.com': {'from_poi_to_this_person': 0}, 'carl.tricoli@enron.com': {'from_poi_to_this_person': 0}, 'heather.kroll@enron.com': {'from_poi_to_this_person': 1}, 'michael.l.miller@enron.com': {'from_poi_to_this_person': 0}, 'thomas.martin@enron.com': {'from_poi_to_this_person': 2}, 'tammie.schoppe@enron.com': {'from_poi_to_this_person': 0}, 'matthew.scrimshaw@enron.com': {'from_poi_to_this_person': 0}, 'g.garcia@enron.com': {'from_poi_to_this_person': 0}, 'MeetingwithJanet': {'from_poi_to_this_person': 0}, 'Billy': {'from_poi_to_this_person': 0}, 'mike.mcconnell@enron.com': {'from_poi_to_this_person': 1}, 'Coal&EmissionsQBRMcClellan': {'from_poi_to_this_person': 0}, 'kent.densley@enron.com': {'from_poi_to_this_person': 0}, 'brad.alford@enron.com': {'from_poi_to_this_person': 0}, 'amy.spoede@enron.com': {'from_poi_to_this_person': 1}, 'wade.cline@enron.com': {'from_poi_to_this_person': 0}, 'john.weakly@enron.com': {'from_poi_to_this_person': 0}, 'ben.glisan@enron.com': {'from_poi_to_this_person': 1}, 'jane.allen@enron.com': {'from_poi_to_this_person': 1}, 'julia.murray@enron.com': {'from_poi_to_this_person': 0}, 'sylvia.hu@enron.com': {'from_poi_to_this_person': 0}, 'beverly.aden@enron.com': {'from_poi_to_this_person': 0}, 'w..pereira@enron.com': {'from_poi_to_this_person': 0}, 'jennifer.fraser@enron.com': {'from_poi_to_this_person': 2}, 'harold.buchanan@enron.com': {'from_poi_to_this_person': 2}, 'barbara.hueter@enron.com': {'from_poi_to_this_person': 1}, 'julie.pechersky@enron.com': {'from_poi_to_this_person': 0}, 'doug.gilbert-smith@enron.com': {'from_poi_to_this_person': 4}, 'richard.shapiro@enron.com': {'from_poi_to_this_person': 6}, 'joseph.sutton@enron.com': {'from_poi_to_this_person': 4}, 'sharon.dick@enron.com': {'from_poi_to_this_person': 0}, 'gwolfe@enron.com': {'from_poi_to_this_person': 0}, 'carol.moffett@enron.com': {'from_poi_to_this_person': 0}, 'michael.brown@enron.com': {'from_poi_to_this_person': 0}, 'michael.mcdonald@enron.com': {'from_poi_to_this_person': 1}, 'PensandPlanesCc': {'from_poi_to_this_person': 1}, 'kimberly.hillis@enron.com': {'from_poi_to_this_person': 0}, 'sean.keenan@enron.com': {'from_poi_to_this_person': 0}, 'webb.jennings@enron.com': {'from_poi_to_this_person': 0}, 'lucy.marshall@enron.com': {'from_poi_to_this_person': 0}, 'edward.baughman@enron.com': {'from_poi_to_this_person': 3}, 'jsteffes@enron.com': {'from_poi_to_this_person': 0}, 'erin.willis@enron.com': {'from_poi_to_this_person': 0}, 'maria.lebeau@enron.com': {'from_poi_to_this_person': 0}, 'navigator@nisource.com': {'from_poi_to_this_person': 0}, 'susan.worthen@enron.com': {'from_poi_to_this_person': 1}, 'richard.sanders@enron.com': {'from_poi_to_this_person': 1}, 'mary.garza@enron.com': {'from_poi_to_this_person': 0}, 'jennifer.reside@enron.com': {'from_poi_to_this_person': 0}, 'tom.communications@enron.com': {'from_poi_to_this_person': 0}, 'james.ajello@enron.com': {'from_poi_to_this_person': 2}, 'robert.greer@enron.com': {'from_poi_to_this_person': 0}, 'carrie.southard@enron.com': {'from_poi_to_this_person': 0}, 'Frevert': {'from_poi_to_this_person': 0}, 'janet.dietrich@enron.com': {'from_poi_to_this_person': 38}, 'mark.haedicke@enron.com': {'from_poi_to_this_person': 17}, 'inez.dauterive@enron.com': {'from_poi_to_this_person': 0}, 'geneva.holland@enron.com': {'from_poi_to_this_person': 0}, 'ina.rangel@enron.com': {'from_poi_to_this_person': 0}, 'jeffrey.miller@enron.com': {'from_poi_to_this_person': 0}, 'Lemmonsre': {'from_poi_to_this_person': 0}, 'rosalee.fleming@enron.com': {'from_poi_to_this_person': 0}, 'luchas.johnson@enron.com': {'from_poi_to_this_person': 0}, 'Helfrich': {'from_poi_to_this_person': 0}, 'kate.cole@enron.com': {'from_poi_to_this_person': 0}, 'eric.scott@enron.com': {'from_poi_to_this_person': 0}, 'selena.gonzalez@enron.com': {'from_poi_to_this_person': 0}, 'david.parquet@enron.com': {'from_poi_to_this_person': 6}, 'stephanie.segura@enron.com': {'from_poi_to_this_person': 0}, 'scott.tholan@enron.com': {'from_poi_to_this_person': 5}, 'james.bouillion@enron.com': {'from_poi_to_this_person': 0}, 'dolores.fisher@enron.com': {'from_poi_to_this_person': 0}, 'Dave': {'from_poi_to_this_person': 0}, 'thomas.white@enron.com': {'from_poi_to_this_person': 0}, 'frank.vickers@enron.com': {'from_poi_to_this_person': 8}, 'john.lavorato@enron.com': {'from_poi_to_this_person': 30}, 'john.hodge@enron.com': {'from_poi_to_this_person': 0}, 'andrew.kelemen@enron.com': {'from_poi_to_this_person': 0}, 'jeffrey.mcmahon@enron.com': {'from_poi_to_this_person': 0}, 'cynthia.barrow@enron.com': {'from_poi_to_this_person': 0}, 'larry.dallman@enron.com': {'from_poi_to_this_person': 0}, 'beth.perlman@enron.com': {'from_poi_to_this_person': 7}, 'steven.kean@enron.com': {'from_poi_to_this_person': 4}, 'shirley.tijerina@enron.com': {'from_poi_to_this_person': 0}, 'mark.muller@enron.com': {'from_poi_to_this_person': 2}, 'raimund.grube@enron.com': {'from_poi_to_this_person': 0}, 'rob.milnthorpe@enron.com': {'from_poi_to_this_person': 0}, 'dick.westfahl@enron.com': {'from_poi_to_this_person': 9}, 'george.carrick@enron.com': {'from_poi_to_this_person': 0}, 'lanette.earnest@enron.com': {'from_poi_to_this_person': 0}, 'cindy.olson@enron.com': {'from_poi_to_this_person': 0}, 'susan.fallon@enron.com': {'from_poi_to_this_person': 0}, 'gordon.mckillop@enron.com': {'from_poi_to_this_person': 0}, 'roger.ondreko@enron.com': {'from_poi_to_this_person': 0}, 'jean.mrha@enron.com': {'from_poi_to_this_person': 13}, 'margaret.daffin@enron.com': {'from_poi_to_this_person': 1}, 'chuck.ames@enron.com': {'from_poi_to_this_person': 0}, 'banu.ozcan@enron.com': {'from_poi_to_this_person': 0}, 'bill.peterson@enron.com': {'from_poi_to_this_person': 0}, 'christopher.f.calger@enron.com': {'from_poi_to_this_person': 0}, 'craig.breslau@enron.com': {'from_poi_to_this_person': 0}, 'ward.polzin@enron.com': {'from_poi_to_this_person': 1}, 'eva.rainer@enron.com': {'from_poi_to_this_person': 0}, 'james.noles@enron.com': {'from_poi_to_this_person': 0}, 'all.houston@enron.com': {'from_poi_to_this_person': 0}, 'calgary.reception@enron.com': {'from_poi_to_this_person': 1}, 'mark.frevert@enron.com': {'from_poi_to_this_person': 17}, 'john.nowlan@enron.com': {'from_poi_to_this_person': 0}, 'james.bannantine@enron.com': {'from_poi_to_this_person': 7}, 'mark.davis@enron.com': {'from_poi_to_this_person': 0}, 'QBRGRMNewProducts&Weather': {'from_poi_to_this_person': 0}, 'MarkDobler': {'from_poi_to_this_person': 0}, 'judy.townsend@enron.com': {'from_poi_to_this_person': 0}, 'scott.josey@enron.com': {'from_poi_to_this_person': 16}, 'jeffrey.hodge@enron.com': {'from_poi_to_this_person': 0}, 'james.steffes@enron.com': {'from_poi_to_this_person': 14}, 'dorothy.dalton@enron.com': {'from_poi_to_this_person': 0}, 'ron.baker@enron.com': {'from_poi_to_this_person': 0}, 'frazier.king@enron.com': {'from_poi_to_this_person': 0}, 'reza.rezaeian@enron.com': {'from_poi_to_this_person': 0}, 'jody.crook@enron.com': {'from_poi_to_this_person': 0}, 'molly.bobrow@enron.com': {'from_poi_to_this_person': 0}, 'greek.rice@enron.com': {'from_poi_to_this_person': 0}, 'tom.hoatson@enron.com': {'from_poi_to_this_person': 0}, 'jurgen.hess@enron.com': {'from_poi_to_this_person': 0}, 'beena.pradhan@enron.com': {'from_poi_to_this_person': 0}, 'nicki.daw@enron.com': {'from_poi_to_this_person': 0}, 'hunter.shively@enron.com': {'from_poi_to_this_person': 5}, 'george.mccormick@enron.com': {'from_poi_to_this_person': 0}, 'mark.frank@enron.com': {'from_poi_to_this_person': 1}, 'michael.driscoll@enron.com': {'from_poi_to_this_person': 0}, 'clay.spears@enron.com': {'from_poi_to_this_person': 2}, 'kyle.kitagawa@enron.com': {'from_poi_to_this_person': 1}, 'CraigFox': {'from_poi_to_this_person': 0}, 'brandon.wax@enron.com': {'from_poi_to_this_person': 0}, 'jonathan.mckay@enron.com': {'from_poi_to_this_person': 1}, 'fran.mayes@enron.com': {'from_poi_to_this_person': 1}, 'joannie.williamson@enron.com': {'from_poi_to_this_person': 0}, 'bruce.sukaly@enron.com': {'from_poi_to_this_person': 1}, 'sheila.tweed@enron.com': {'from_poi_to_this_person': 0}, 'dan.leff@enron.com': {'from_poi_to_this_person': 4}, 'matthew.jachimiak@enron.com': {'from_poi_to_this_person': 0}, 'stephen.douglass@enron.com': {'from_poi_to_this_person': 1}, 'chris.meyer@enron.com': {'from_poi_to_this_person': 0}, 'mrudula.gadade@enron.com': {'from_poi_to_this_person': 0}, 'frank.hoogendoorn@enron.com': {'from_poi_to_this_person': 0}, 'jennifer.n.stewart@enron.com': {'from_poi_to_this_person': 0}, 'duncan.croasdale@enron.com': {'from_poi_to_this_person': 0}, 'dave.kellermeyer@enron.com': {'from_poi_to_this_person': 0}, 'nella.cappelletto@enron.com': {'from_poi_to_this_person': 1}, 'brad.blesie@enron.com': {'from_poi_to_this_person': 0}, 'karen.heathman@enron.com': {'from_poi_to_this_person': 0}, 'rebecca.carter@enron.com': {'from_poi_to_this_person': 0}, 'brent.price@enron.com': {'from_poi_to_this_person': 0}, 'mike.curry@enron.com': {'from_poi_to_this_person': 3}, 'purvi.patel@enron.com': {'from_poi_to_this_person': 0}, 'john.scarborough@enron.com': {'from_poi_to_this_person': 0}, 'greg.wolfe@enron.com': {'from_poi_to_this_person': 3}, 'sharron.westbrook@enron.com': {'from_poi_to_this_person': 0}, 'brad.carey@enron.com': {'from_poi_to_this_person': 0}, 'ross.prevatt@enron.com': {'from_poi_to_this_person': 0}, 'Beyer': {'from_poi_to_this_person': 0}, 'richard.lydecker@enron.com': {'from_poi_to_this_person': 4}, 'chris.mallory@enron.com': {'from_poi_to_this_person': 0}, 'martina.angelova@enron.com': {'from_poi_to_this_person': 0}, 'robert.virgo@enron.com': {'from_poi_to_this_person': 9}, 'greg.whalley@enron.com': {'from_poi_to_this_person': 3}, 'dorie.hitchcock@enron.com': {'from_poi_to_this_person': 6}, 'larry.izzo@enron.com': {'from_poi_to_this_person': 5}}

#### Code up a new feature

Given:

from_this_person_to_poi

from_poi_to_this_person

from_messages

to_messages

Get:

fraction_from_poi_to_this_person

fraction_from_this_person_to_poi

```Python
# code used for submission on Udacity

import pickle
from get_data import getData

def computeFraction( poi_messages, all_messages ):
    """ given a number messages to/from POI (numerator)
        and number of all messages to/from a person (denominator),
        return the fraction of messages to/from that person
        that are from/to a POI
   """
    ### you fill in this code, so that it returns either
    ###     the fraction of all messages to this person that come from POIs
    ###     or
    ###     the fraction of all messages from this person that are sent to POIs
    ### the same code can be used to compute either quantity

    ### beware of "NaN" when there is no known email address (and so
    ### no filled email features), and integer division!
    ### in case of poi_messages or all_messages having "NaN" value, return 0.
    fraction = 0
    if poi_messages == 'NaN' or all_messages == 'NaN':
        fraction = 0
    if poi_messages != 'NaN' and all_messages != 'NaN':
        fraction = float(poi_messages)/float(all_messages)
    return fraction

data_dict = getData()

submit_dict = {}
for name in data_dict:
    data_point = data_dict[name]
    print
    from_poi_to_this_person = data_point["from_poi_to_this_person"]
    to_messages = data_point["to_messages"]
    fraction_from_poi = computeFraction( from_poi_to_this_person, to_messages )
    print fraction_from_poi
    data_point["fraction_from_poi"] = fraction_from_poi

    from_this_person_to_poi = data_point["from_this_person_to_poi"]
    from_messages = data_point["from_messages"]
    fraction_to_poi = computeFraction( from_this_person_to_poi, from_messages )
    print fraction_to_poi
    submit_dict[name]={"from_poi_to_this_person":fraction_from_poi,
                       "from_this_person_to_poi":fraction_to_poi}
    data_point["fraction_to_poi"] = fraction_to_poi

#####################

def submitDict():
    return submit_dict
```

#### Buggy feature
Beware of bugs when creating new features.

When Katie was working on the Enron POI identifier, she engineered a feature that identified when a given person was on the same email as a POI. So for example, if Ken Lay and Katie Malone are both recipients of the same email message, then Katie Malone should have her "shared receipt" feature incremented. If she shares lots of emails with POIs, maybe she's a POI herself.

Here's the problem: there was a subtle bug, that Ken Lay's "shared receipt" counter would also be incremented when this happens. And of course, then Ken Lay always shares receipt with a POI, because he is a POI. So the "shared receipt" feature became extremely powerful in finding POIs, because it effectively was encoding the label for each person as a feature.

We found this first by being suspicious of a classifier that was always returning 100% accuracy. Then we removed features one at a time, and found that this feature was driving all the performance. Then, digging back through the feature code, we found the bug outlined above. We changed the code so that a person's "shared receipt" feature was only incremented if there was a different POI who received the email, reran the code, and tried again. The accuracy dropped to a more reasonable level.

We take a couple of lessons from this:

Anyone can make mistakes--be skeptical of your results!
100% accuracy should generally make you suspicious. Extraordinary claims require extraordinary proof.
If there's a feature that tracks your labels a little too closely, it's very likely a bug!
If you're sure it's not a bug, you probably don't need machine learning--you can just use that feature alone to assign labels.

#### Univariate feature selection

There are several go-to methods of automatically selecting your features in sklearn. Many of them fall under the umbrella of univariate feature selection, which treats each feature independently and asks how much power it gives you in classifying or regressing.

There are two big univariate feature selection tools in sklearn: SelectPercentile and SelectKBest. The difference is pretty apparent by the names: **SelectPercentile selects the X% of features that are most powerful (where X is a parameter) and SelectKBest selects the K features that are most powerful (where K is a parameter).**

A clear candidate for feature reduction is text learning, since the data has such high dimension. We actually did feature selection in the Sara/Chris email classification problem during the first few mini-projects; you can see it in the code in tools/email_preprocess.py .

``` python

from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.feature_selection import SelectPercentile, f_classif

    ### test_size is the percentage of events assigned to the test set
    ### (remainder go into training)
    features_train, features_test, labels_train, labels_test = cross_validation.train_test_split(word_data, authors, test_size=0.1, random_state=42)


    ### text vectorization--go from strings to lists of numbers
    vectorizer = TfidfVectorizer(sublinear_tf=True, max_df=0.5,
                                 stop_words='english')  #max_df--maximum document frequency
    features_train_transformed = vectorizer.fit_transform(features_train)
    features_test_transformed  = vectorizer.transform(features_test)


### feature selection, because text is super high dimensional and can be really computationally chewy as a result
    selector = SelectPercentile(f_classif, percentile=10)
    selector.fit(features_train_transformed, labels_train)
    features_train_transformed = selector.transform(features_train_transformed).toarray()
    features_test_transformed  = selector.transform(features_test_transformed).toarray()
```

#### Regularization in Regression

Using Lasso in Sklearn: http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.Lasso.html


```Python

import sklearn.linear_model.Lasso
features,labels = GetMyData()
regression = Lasso()
regression.fit(features, labels)

# To predict a label for a new point
regression.predict([2,4])

# To view the coefficients and intercept of this regression
print (regression.coef_)

print (regression.intercept_)

```



FYI, the documentation requests an array of shape = (n_samples, n_features). It happens in this case that sklearn is lenient and will still work if you give it a one dimensional array of features and you want a single prediction, but the answer regression.predict([[2,4]]) is a little more correct.

### Feature Selection Mini-Project

Katie explained in a video a problem that arose in preparing Chris and Sara’s email for the author identification project; it had to do with a feature that was a little too powerful (effectively acting like a signature, which gives an arguably unfair advantage to an algorithm). You’ll work through that discovery process here.

This bug was found when Katie was trying to make an overfit decision tree to use as an example in the decision tree mini-project. A decision tree is classically an algorithm that can be easy to overfit; one of the easiest ways to get an overfit decision tree is to use a small training set and lots of features.
If a decision tree is overfit, would you expect the accuracy on a test set to be very high or pretty low?

Answer:  very low.

If a decision tree is overfit, would you expect high or low accuracy on the training set?

Answer: The accuracy would be very high on the training set, but would plummet once it was actually tested.

A classic way to overfit an algorithm is by using lots of features and not a lot of training data. You can find the starter code in feature_selection/find_signature.py. Get a decision tree up and training on the training data, and print out the accuracy. How many training points are there, according to the starter code?


Quiz: Number Of Features And Overfitting
Special Note: Depending on when you downloaded the code provided for **find_signature.py**, you may need to change the code in lines 9-10 to be
**words_file = "../text_learning/your_word_data.pkl" authors_file = "../text_learning/your_email_authors.pkl"**
so that the files created from running **vectorize_text.py** are reflected properly.

In addition, if you are having trouble getting the code to run due to memory issues, then if you are on version 0.16.x of scikit-learn, you can remove the .toarray() function from the line where features_train is created to save on memory - the decision tree classifier can, in that version take as input a sparse array instead of only dense arrays.



```python
#!/usr/bin/python

import pickle
import numpy
numpy.random.seed(42)


### The words (features) and authors (labels), already largely processed.
### These files should have been created from the previous (Lesson 10)
### mini-project.
words_file = "your_word_data1.pkl"
authors_file = "your_email_authors1.pkl"
word_data = pickle.load( open(words_file, "r"))
authors = pickle.load( open(authors_file, "r") )


### test_size is the percentage of events assigned to the test set (the
### remainder go into training)
### feature matrices changed to dense representations for compatibility with
### classifier functions in versions 0.15.2 and earlier
from sklearn import cross_validation
features_train, features_test, labels_train, labels_test = cross_validation.train_test_split(word_data, authors, test_size=0.1, random_state=42)

from sklearn.feature_extraction.text import TfidfVectorizer
vectorizer = TfidfVectorizer(sublinear_tf=True, max_df=0.5,
                             stop_words='english')
features_train = vectorizer.fit_transform(features_train)
features_test  = vectorizer.transform(features_test).toarray()


### a classic way to overfit is to use a small number
### of data points and a large number of features;
### train on only 150 events to put ourselves in this regime
features_train = features_train[:150].toarray()
labels_train   = labels_train[:150]

```


```python
# We've limited our training data quite a bit, so we should be expecting our models to potentially overfit.
len(features_train)
```




    150




```python
# What’s the accuracy of the decision tree you just made?
# (Remember, we're setting up our decision tree to overfit -- ideally, we want to see the test accuracy as relatively low.)
from sklearn.metrics import accuracy_score
from sklearn import tree
clf = tree.DecisionTreeClassifier()
clf = clf.fit(features_train, labels_train)
pred = clf.predict(features_test)
acc = accuracy_score(pred, labels_test)
print acc
```

    0.947667804323



```python
clf.score(features_test, labels_test)
```




    0.94766780432309439



Yes, the test performance has an accuracy much higher than it is expected to be - if we are overfitting, then the test performance should be relatively low.

#### Identify the Most Powerful Features

Take your (overfit) decision tree and use the **```feature_importances_```** attribute to get a list of the relative importance of all the features being used. We suggest iterating through this list (it’s long, since this is text data) and only printing out the feature importance if it’s above some threshold (say, 0.2--remember, if all words were equally important, each one would give an importance of far less than 0.01). What’s the importance of the most important feature? What is the number of this feature?

http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html


```python
max(clf.feature_importances_)
```




    0.76470588235294124




```python
len(clf.feature_importances_[clf.feature_importances_ > 0.2])
```




    1




```python
a = 0
max_importance = max(clf.feature_importances_)
for i in range(len(clf.feature_importances_)):
    if clf.feature_importances_[i] == max_importance:
        a = i
a
```




    33614



#### Use TfIdf to Get the Most Important Word
In order to figure out what words are causing the problem, you need to go back to the TfIdf and use the feature numbers that you obtained in the previous part of the mini-project to get the associated words. You can return a list of all the words in the TfIdf by calling get_feature_names() on it; pull out the word that’s causing most of the discrimination of the decision tree. What is it? Does it make sense as a word that’s uniquely tied to either Chris Germany or Sara Shackleton, a signature of sorts?


```python
new_list = vectorizer.get_feature_names()
new_list[33614]

#Even though our training data is limited, we still have a word that is highly indicative of author.
```




    u'sshacklensf'



#### Remove, Repeat

This word seems like an outlier in a certain sense, so let’s remove it and refit. Go back to **text_learning/vectorize_text.py**, and remove this word from the emails using the same method you used to remove “sara”, “chris”, etc. Rerun **vectorize_text.py**, and once that finishes, rerun **find_signature.py**. Any other outliers pop up? What word is it? Seem like a signature-type word? (Define an outlier as a feature with importance >0.2, as before).

##### Get the new data file (with outlier being removed) from the Text_Learning_Mini_Project


```python
import pickle
import numpy
numpy.random.seed(42)


### The words (features) and authors (labels), already largely processed.
### These files should have been created from the previous (Lesson 10)
### mini-project.
words_file = "your_word_data.pkl"
authors_file = "your_email_authors.pkl"
word_data = pickle.load( open(words_file, "r"))
authors = pickle.load( open(authors_file, "r") )


### test_size is the percentage of events assigned to the test set (the
### remainder go into training)
### feature matrices changed to dense representations for compatibility with
### classifier functions in versions 0.15.2 and earlier
from sklearn import cross_validation
features_train, features_test, labels_train, labels_test = cross_validation.train_test_split(word_data, authors, test_size=0.1, random_state=42)

from sklearn.feature_extraction.text import TfidfVectorizer
vectorizer = TfidfVectorizer(sublinear_tf=True, max_df=0.5,
                             stop_words='english')
features_train = vectorizer.fit_transform(features_train)
features_test  = vectorizer.transform(features_test).toarray()


### a classic way to overfit is to use a small number
### of data points and a large number of features;
### train on only 150 events to put ourselves in this regime
features_train = features_train[:150].toarray()
labels_train   = labels_train[:150]
```


```python
from sklearn.metrics import accuracy_score
from sklearn import tree
clf = tree.DecisionTreeClassifier()
clf = clf.fit(features_train, labels_train)
len(clf.feature_importances_[clf.feature_importances_ > 0.2])
```




    1




```python
b = 0
max_importance = max(clf.feature_importances_)
for i in range(len(clf.feature_importances_)):
    if clf.feature_importances_[i] == max_importance:
        b = i
b
```




    14343




```python
new_list = vectorizer.get_feature_names()
new_list[14343]

# Yes, this is the next most "highly powerful" word.
```




    u'cgermannsf'



#### Checking Important Features Again

Update vectorize_test.py one more time, and rerun. Then run find_signature.py again. Any other important features (importance>0.2) arise? How many? Do any of them look like “signature words”, or are they more “email content” words, that look like they legitimately come from the text of the messages?


```python
#
import pickle
import numpy
numpy.random.seed(42)


### The words (features) and authors (labels), already largely processed.
### These files should have been created from the previous (Lesson 10)
### mini-project.
words_file = "your_word_data3.pkl"
authors_file = "your_email_authors3.pkl"
word_data = pickle.load( open(words_file, "r"))
authors = pickle.load( open(authors_file, "r") )


### test_size is the percentage of events assigned to the test set (the
### remainder go into training)
### feature matrices changed to dense representations for compatibility with
### classifier functions in versions 0.15.2 and earlier
from sklearn import cross_validation
features_train, features_test, labels_train, labels_test = cross_validation.train_test_split(word_data, authors, test_size=0.1, random_state=42)

from sklearn.feature_extraction.text import TfidfVectorizer
vectorizer = TfidfVectorizer(sublinear_tf=True, max_df=0.5,
                             stop_words='english')
features_train = vectorizer.fit_transform(features_train)
features_test  = vectorizer.transform(features_test).toarray()


### a classic way to overfit is to use a small number
### of data points and a large number of features;
### train on only 150 events to put ourselves in this regime
features_train = features_train[:150].toarray()
labels_train   = labels_train[:150]
```


```python
from sklearn.metrics import accuracy_score
from sklearn import tree
clf = tree.DecisionTreeClassifier()
clf = clf.fit(features_train, labels_train)
len(clf.feature_importances_[clf.feature_importances_ > 0.2])
```




    1




```python
c = 0
max_importance = max(clf.feature_importances_)
for i in range(len(clf.feature_importances_)):
    if clf.feature_importances_[i] == max_importance:
        c = i
c
```




    21323




```python
new_list = vectorizer.get_feature_names()
new_list[21323]
```




    u'houectect'



Yes, there is one more word ("houectect").  Your guess about what this word means is as good as ours, but it doesn't look like an obvious signature word so let's keep moving without removing it.

Signature words:  ["sara", "shackleton", "chris", "germani"]

#### Accuracy of the Overfit Tree

What’s the accuracy of the decision tree now? We've removed two "signature words", so it will be more difficult for the algorithm to fit to our limited training set without overfitting. Remember, the whole point was to see if we could get the algorithm to overfit--a sensible result is one where the accuracy isn't that great!


```python
pred = clf.predict(features_test)
acc = accuracy_score(pred, labels_test)
print acc
```

    0.816837315131


Now that we've removed the outlier "signature words", the training data is starting to overfit to the words that remain.
