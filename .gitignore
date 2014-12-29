#!/bin/bash

WORKDIR="/diretorio/"

SESSION="inc"-$(date  +%Y%m%d%H%M%S)
echo "SESSION: $SESSION started on $(date)" >> $WORKDIR/sessions.txt
data=`date +%m/%d/%y --date="-2 day"`
pasta_session="$WORKDIR/$SESSION"
mkdir -p $pasta_session

LISTALLACCOUNTS=$(mktemp)
ldapsearch -D "cn=config" -w senha_master_ldap -p 389 -h IP_servidorldap -b '' -LLL "(objectclass=zimbraAccount)" zimbraMailDeliveryAddress zimbraMailHost | grep ^zimbraMail | awk '{print $2}' > "$LISTALLACCOUNTS"

for MAIL in $(grep @ $LISTALLACCOUNTS); do
        $(which curl) -k -u admin:senhaadmin https://enderecoservidocorreio:7071/home/$MAIL/?fmt=tgz\&query=after:$data > $pasta_session/$MAIL.tgz
        $(which ldapsearch) -D "cn=config" -w senha_master_ldap -p 389 -h IP_servidorldap -b '' -LLL "(zimbraMailDeliveryAddress=$MAIL)" > $pasta_session/$MAIL.ldiff
        echo $SESSION:$MAIL:$(date +%m/%d/%y) >> $WORKDIR/sessions.txt
done

chown -R zimbra:zimbra $pasta_session

echo "SESSION: $SESSION finished on $(date)" >> $WORKDIR/sessions.txt
