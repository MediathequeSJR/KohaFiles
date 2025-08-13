<!DOCTYPE stylesheet [<!ENTITY nbsp "&#160;" >]>

<!-- $Id: MARC21slim2DC.xsl,v 1.1 2003/01/06 08:20:27 adam Exp $ -->
<!-- modifié par Marjorie 17 mars 2015-->
<!--ajout 340 26/06/2019-->
<!--correction 245 18/02/2022-->

<xsl:stylesheet version="1.0"
  xmlns:marc="http://www.loc.gov/MARC21/slim"
  xmlns:items="http://www.koha-community.org/items"
  xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
  exclude-result-prefixes="marc items">
 <xsl:import href="http://stylesheets.koha.collecto.ca/MARC21slimUtils.xsl"/>
 <xsl:output method = "html" indent="yes" omit-xml-declaration = "yes" encoding="UTF-8"/>
 <xsl:template match="/">
 <xsl:apply-templates/>
 </xsl:template>

 <xsl:template match="marc:record">

  <!-- Option: Display Alternate Graphic Representation (MARC 880)  -->
        <xsl:variable name="display880" select="boolean(marc:datafield[@tag=880])"/>
        <xsl:variable name="UseControlNumber" select="marc:sysprefs/marc:syspref[@name='UseControlNumber']"/>
        <xsl:variable name="URLLinkText" select="marc:sysprefs/marc:syspref[@name='URLLinkText']"/>
        <xsl:variable name="OPACBaseURL" select="marc:sysprefs/marc:syspref[@name='OPACBaseURL']"/>
        <xsl:variable name="SubjectModifier"><xsl:if test="marc:sysprefs/marc:syspref[@name='TraceCompleteSubfields']='1'">,complete-subfield</xsl:if></xsl:variable>
        <xsl:variable name="UseAuthoritiesForTracings" select="marc:sysprefs/marc:syspref[@name='UseAuthoritiesForTracings']"/>
        <xsl:variable name="TraceSubjectSubdivisions" select="marc:sysprefs/marc:syspref[@name='TraceSubjectSubdivisions']"/>
        <xsl:variable name="Show856uAsImage" select="marc:sysprefs/marc:syspref[@name='Display856uAsImage']"/>
        <xsl:variable name="DisplayIconsXSLT" select="marc:sysprefs/marc:syspref[@name='DisplayIconsXSLT']"/>
        <xsl:variable name="OpacSuppression" select="marc:sysprefs/marc:syspref[@name='OpacSuppression']"/>
        <xsl:variable name="TracingQuotesLeft">
           <xsl:choose>
             <xsl:when test="marc:sysprefs/marc:syspref[@name='UseICU']='1'">{</xsl:when>
             <xsl:otherwise>"</xsl:otherwise>
           </xsl:choose>
        </xsl:variable>
        <xsl:variable name="TracingQuotesRight">
          <xsl:choose>
            <xsl:when test="marc:sysprefs/marc:syspref[@name='UseICU']='1'">}</xsl:when>
            <xsl:otherwise>"</xsl:otherwise>
          </xsl:choose>
        </xsl:variable>

        <xsl:variable name="leader" select="marc:leader"/>
        <xsl:variable name="leader6" select="substring($leader,7,1)"/>
        <xsl:variable name="leader7" select="substring($leader,8,1)"/>
        <xsl:variable name="leader19" select="substring($leader,20,1)"/>
        <xsl:variable name="controlField008" select="marc:controlfield[@tag=008]"/>
        <xsl:variable name="materialTypeCode">
            <xsl:choose>
                <xsl:when test="$leader19='a'">ST</xsl:when>
                <xsl:when test="$leader6='a'">
                    <xsl:choose>
                        <xsl:when test="$leader7='c' or $leader7='d' or $leader7='m'">BK</xsl:when>
                        <xsl:when test="$leader7='i' or $leader7='s'">SE</xsl:when>
                        <xsl:when test="$leader7='a' or $leader7='b'">AR</xsl:when>
                    </xsl:choose>
                </xsl:when>
                <xsl:when test="$leader6='t'">BK</xsl:when>
                <xsl:when test="$leader6='o' or $leader6='p'">MX</xsl:when>
                <xsl:when test="$leader6='m'">CF</xsl:when>
                <xsl:when test="$leader6='e' or $leader6='f'">MP</xsl:when>
                <xsl:when test="$leader6='g'">VM</xsl:when>
                <xsl:when test="$leader6='k'">PK</xsl:when>
                <xsl:when test="$leader6='r'">OB</xsl:when>
                <xsl:when test="$leader6='i'">SO</xsl:when>
                <xsl:when test="$leader6='j'">MU</xsl:when>
                <xsl:when test="$leader6='c' or $leader6='d'">PR</xsl:when>
            </xsl:choose>
        </xsl:variable>
        <xsl:variable name="materialTypeLabel">
            <xsl:choose>
                <xsl:when test="$leader19='a'">Set</xsl:when>
                <xsl:when test="$leader6='a'">
                    <xsl:choose>
                        <xsl:when test="$leader7='c' or $leader7='d' or $leader7='m'">Livre</xsl:when>
                        <xsl:when test="$leader7='i' or $leader7='s'">
                            <xsl:choose>
                                <xsl:when test="substring($controlField008,22,1)!='m'">Continuing resource</xsl:when>
                                <xsl:otherwise>Series</xsl:otherwise>
                            </xsl:choose>
                        </xsl:when>
                        <xsl:when test="$leader7='a' or $leader7='b'">Article</xsl:when>
                    </xsl:choose>
                </xsl:when>
                <xsl:when test="$leader6='t'"> Livre</xsl:when>
				<xsl:when test="$leader6='o'"> Kit</xsl:when>
                <xsl:when test="$leader6='p'"> Matériel mixte</xsl:when>
                <xsl:when test="$leader6='m'"> Fichier informatique</xsl:when>
                <xsl:when test="$leader6='e' or $leader6='f'"> Carte</xsl:when>
                <xsl:when test="$leader6='g'"> Film</xsl:when>
                <xsl:when test="$leader6='k'"> Image</xsl:when>
                <xsl:when test="$leader6='r'"> Objet</xsl:when>
                <xsl:when test="$leader6='j'"> Musique</xsl:when>
                <xsl:when test="$leader6='i'"> Son</xsl:when>
                <xsl:when test="$leader6='c' or $leader6='d'"> Partition</xsl:when>
            </xsl:choose>
        </xsl:variable>

 <!-- Indicate if record is suppressed in OPAC -->
        <xsl:if test="$OpacSuppression = 1">
            <xsl:if test="marc:datafield[@tag=942][marc:subfield[@code='n'] = '1']">
                <span class="results_summary suppressed_opac">Caché à l'OPAC</span>
            </xsl:if>
        </xsl:if>

  <!-- Title Statement -->
        <!-- Alternate Graphic Representation (MARC 880) -->
       <xsl:if test="$display880">
            <h1>
                <xsl:call-template name="m880Select">
                    <xsl:with-param name="basetags">245</xsl:with-param>
                    <xsl:with-param name="codes">abhfgknps</xsl:with-param>
                </xsl:call-template>
            </h1>
        </xsl:if>

  <!--Bug 13381 -->
            <xsl:if test="marc:datafield[@tag=245]">
                <h1>
                    <xsl:for-each select="marc:datafield[@tag=245]">
                        <xsl:call-template name="subfieldSelect">
                            <xsl:with-param name="codes">a</xsl:with-param>
                        </xsl:call-template>
                        <xsl:text> </xsl:text>
                        <!-- 13381 add additional subfields-->
                        <!-- bug17625 adding f and g subfields -->
                        <xsl:for-each select="marc:subfield[contains('bchknps', @code)]">
                            <xsl:choose>
                                <xsl:when test="@code='h'"> 
                                    <!--  13381 Span class around subfield h so it can be suppressed via css -->
                                    <span class="title_medium"><xsl:apply-templates/></span>
									<xsl:text> </xsl:text>						   
                                </xsl:when>
                                <xsl:when test="@code='c'">
                                    <!--  13381 Span class around subfield c so it can be suppressed via css -->
                                    <span class="title_resp_stmt"><xsl:apply-templates/> </span>
                                </xsl:when>
                                <xsl:otherwise>
                                    <xsl:apply-templates/>
                                    <xsl:text> </xsl:text>
                                </xsl:otherwise>
                            </xsl:choose>
                        </xsl:for-each>
                    </xsl:for-each>
                </h1>
            </xsl:if>
 

 <xsl:if test="$DisplayIconsXSLT!='0' and $materialTypeCode!=''">
        <span class="results_summary type"><span class="label">Type: </span>
    <xsl:element name="img"><xsl:attribute name="class">materialtype mt_icon_<xsl:value-of select="$materialTypeCode"/></xsl:attribute><xsl:attribute name="src">/intranet-tmpl/prog/img/famfamfam/<xsl:value-of select="$materialTypeCode"/>.png</xsl:attribute><xsl:attribute name="alt"></xsl:attribute></xsl:element>
        <xsl:text> </xsl:text>
        <xsl:value-of select="$materialTypeLabel"/>
        </span>
    </xsl:if>
 
 <!-- Author Statement: Alternate Graphic Representation (MARC 880) -->
 <xsl:if test="$display880">
 <h5 class="author">
 <xsl:call-template name="m880Select">
 <xsl:with-param name="basetags">100,110,111,</xsl:with-param>
 <xsl:with-param name="codes">abcdfghijklmnopqrstux</xsl:with-param>
 <xsl:with-param name="index">au</xsl:with-param>
 <!-- do not use label 'by ' here, it would be repeated for every occurence of 100,110,111,700,710,711 -->
 </xsl:call-template>
 </h5>
 </xsl:if>

 <xsl:choose>
 <xsl:when test="marc:datafield[@tag=100] or marc:datafield[@tag=110]">
 <h5 class="author">Auteur : <xsl:call-template name="showAuthor">
 
 <xsl:with-param name="authorfield" select="marc:datafield[@tag=100 or @tag=110]"/>
 <xsl:with-param name="UseAuthoritiesForTracings" select="$UseAuthoritiesForTracings"/>
 
 </xsl:call-template>
 </h5>
 </xsl:when>
 </xsl:choose>
 
 
  <!--111 -->
 <xsl:if test="$display880">
 <h5 class="author">
 <xsl:call-template name="m880Select">
 <xsl:with-param name="basetags">111</xsl:with-param>
 <xsl:with-param name="codes">andc</xsl:with-param>
 <xsl:with-param name="index">au</xsl:with-param>

 </xsl:call-template>
 </h5>
 </xsl:if>

 <xsl:choose>
 <xsl:when test="marc:datafield[@tag=111]">
 <h5 class="author">Auteur : <xsl:call-template name="showAuthor">
 
 <xsl:with-param name="authorfield" select="marc:datafield[@tag=111]"/>
 <xsl:with-param name="UseAuthoritiesForTracings" select="$UseAuthoritiesForTracings"/>
 
 </xsl:call-template>
 </h5>
 </xsl:when>
 </xsl:choose>
 
 
  <!-- 700,710,711 -->
 <xsl:if test="$display880">
<h6><span class="label"></span>
 <xsl:call-template name="m880Select">
 <xsl:with-param name="basetags">700,710,711</xsl:with-param>
 <xsl:with-param name="codes">abcdefghijklmnopqrstuvwxyz</xsl:with-param>
 <xsl:with-param name="index">au</xsl:with-param>
 <!-- do not use label 'by ' here, it would be repeated for every occurence of 100,110,111,700,710,711 -->
 </xsl:call-template>
 </h6>
 </xsl:if>

 
 <xsl:choose>
 <xsl:when test="marc:datafield[@tag=700]">
 <span class="results_summary">Collaborateur(s):<div class="contentblock"> <xsl:call-template name="showAuthor">
 <xsl:with-param name="authorfield" select="marc:datafield[@tag=700]"/>
 <xsl:with-param name="UseAuthoritiesForTracings" select="$UseAuthoritiesForTracings"/>
 </xsl:call-template>
 </div>
 </span>
 </xsl:when>
 </xsl:choose>
 <xsl:choose>
 <xsl:when test="marc:datafield[@tag=710] ">
 <span class="results_summary">Collaborateur(s):<div class="contentblock"> <xsl:call-template name="showAuthor">
 <xsl:with-param name="authorfield" select="marc:datafield[@tag=710]"/>
 <xsl:with-param name="UseAuthoritiesForTracings" select="$UseAuthoritiesForTracings"/>
 </xsl:call-template>
 </div>
 </span>
 </xsl:when>
 </xsl:choose>
 <xsl:choose>
 <xsl:when test="marc:datafield[@tag=711]">
 <span class="results_summary">Collaborateur(s): <div class="contentblock"><xsl:call-template name="showAuthor">
 <xsl:with-param name="authorfield" select="marc:datafield[@tag=711]"/>
 <xsl:with-param name="UseAuthoritiesForTracings" select="$UseAuthoritiesForTracings"/>
 </xsl:call-template>
 </div>
 </span>
 </xsl:when>
 </xsl:choose>


 <!--Series: Alternate Graphic Representation (MARC 880) -->
 <xsl:if test="$display880">
 <xsl:call-template name="m880Select">
 <xsl:with-param name="basetags">440,490</xsl:with-param>
 <xsl:with-param name="codes">axvp</xsl:with-param>
 <xsl:with-param name="class">results_summary series</xsl:with-param>
 <xsl:with-param name="label">Collection&nbsp;:</xsl:with-param>
 <xsl:with-param name="index">se</xsl:with-param>
 </xsl:call-template>
 </xsl:if>
 
 <!-- Series -->
 <xsl:if test="marc:datafield[@tag=440 or @tag=490]">
 <span class="results_summary series"><span class="label">Collection&nbsp;:</span>
 <!-- 440 -->
 <xsl:for-each select="marc:datafield[@tag=440]">
 <a><xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=se,phr:"<xsl:value-of select="marc:subfield[@code='a']"/>"</xsl:attribute>
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">axvp</xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>
 </a>
 <xsl:call-template name="part"/>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text>. </xsl:text></xsl:when><xsl:otherwise><xsl:text> ; </xsl:text></xsl:otherwise></xsl:choose>
 </xsl:for-each>

 <!-- 490 Series not traced, Ind1 = 0 -->
 <xsl:for-each select="marc:datafield[@tag=490][@ind1!=1]">
 <a><xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=se,phr:"<xsl:value-of select="marc:subfield[@code='a']"/>"</xsl:attribute>
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">av</xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>
 </a>
 <xsl:call-template name="part"/>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text>.</xsl:text></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </xsl:for-each>
 <!-- 490 Series traced, Ind1 = 1 -->
 <xsl:if test="marc:datafield[@tag=490][@ind1=1]">
 <xsl:for-each select="marc:datafield[@tag=800 or @tag=810 or @tag=811 or @tag=830]">
 <xsl:choose>
 <xsl:when test="$UseControlNumber = '1' and marc:subfield[@code='w']">
 <a href="/cgi-bin/koha/catalogue/search.pl?q=rcn:{marc:subfield[@code='w']}">
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">a_t</xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>
 </a>
 </xsl:when>
 <xsl:otherwise>
 <a><xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=se,phr:"<xsl:value-of select="marc:subfield[@code='a']"/>"</xsl:attribute>
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">a_t</xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>
 </a>
 <xsl:call-template name="part"/>
 </xsl:otherwise>
 </xsl:choose>
 <xsl:text>: </xsl:text>
 <xsl:value-of  select="marc:subfield[@code='v']" />
 <xsl:choose><xsl:when test="position()=last()"><xsl:text></xsl:text></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </xsl:for-each>
 </xsl:if>

 </span>
 </xsl:if>

 <!-- Analytics  -->
 <xsl:if test="marc:datafield[@tag=774]">
 <span class="results_summary analytics"><span class="label">Notice(s) analytique(s): </span>
 <a>
 <xsl:choose>
 <xsl:when test="$UseControlNumber = '1' and marc:controlfield[@tag=001]">
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=rcn:<xsl:value-of select="marc:controlfield[@tag=001]"/>+and+(bib-level:a+or+bib-level:b)</xsl:attribute>
 </xsl:when>
 <xsl:otherwise>
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=Host-item:<xsl:value-of select="translate(marc:datafield[@tag=245]/marc:subfield[@code='a'], '/', '')"/></xsl:attribute>
 </xsl:otherwise>
 </xsl:choose>
 <xsl:text>Voir les notices</xsl:text>
 </a>
 </span>
 </xsl:if>


 <!-- Volumes of sets and traced series -->
 <xsl:if test="$materialTypeCode='ST' or substring($controlField008,22,1)='m'">
 <span class="results_summary volumes"><span class="label">Volumes&nbsp;: </span>
 <a>
 <xsl:choose>
 <xsl:when test="$UseControlNumber = '1' and marc:controlfield[@tag=001]">
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=rcn:<xsl:value-of select="marc:controlfield[@tag=001]"/>+not+(bib-level:a+or+bib-level:b)</xsl:attribute>
 </xsl:when>
 <xsl:otherwise>
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=ti,phr:<xsl:value-of select="translate(marc:datafield[@tag=245]/marc:subfield[@code='a'], '/', '')"/></xsl:attribute>
 </xsl:otherwise>
 </xsl:choose>
 <xsl:text>Afficher les volumes</xsl:text>
 </a>
 </span>
 </xsl:if>

 <!-- Set -->
 <xsl:if test="$leader19='c'">
 <span class="results_summary set"><span class="label">Groupe&nbsp;: </span>
 <xsl:for-each select="marc:datafield[@tag=773]">
 <a>
 <xsl:choose>
 <xsl:when test="$UseControlNumber = '1' and marc:subfield[@code='w']">
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=sn:<xsl:call-template name="extractControlNumber"><xsl:with-param name="subfieldW" select="marc:subfield[@code='w']"/></xsl:call-template></xsl:attribute>
 </xsl:when>
 <xsl:otherwise>
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=ti,phr:<xsl:value-of select="translate(//marc:datafield[@tag=245]/marc:subfield[@code='a'], '.', '')"/></xsl:attribute>
 </xsl:otherwise>
 </xsl:choose>
 <xsl:value-of select="translate(//marc:datafield[@tag=245]/marc:subfield[@code='a'], '.', '')" />
 </a>
 <xsl:choose>
 <xsl:when test="position()=last()"></xsl:when>
 <xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise>
 </xsl:choose>
 </xsl:for-each>
 </span>
 </xsl:if>

 <!-- Publisher Statement: Alternate Graphic Representation (MARC 880) -->
   <xsl:choose>
 <xsl:when test="marc:datafield[@tag=264]">
 <xsl:if test="$display880">
 <xsl:call-template name="m880Select">
 <xsl:with-param name="basetags">264</xsl:with-param>
 <xsl:with-param name="codes">3abcdefg</xsl:with-param>
 <xsl:with-param name="class">results_summary publisher</xsl:with-param>
 <xsl:with-param name="label"> Éditeur(s) : </xsl:with-param>
 </xsl:call-template>
 </xsl:if>
 <xsl:if test="marc:datafield[@tag=264]">
 <xsl:for-each select="marc:datafield[@tag=264]">
 
 <span class="results_summary publisher">
 <xsl:choose>
 <xsl:when test="@ind2=0"><xsl:text>Production : </xsl:text></xsl:when>
 <xsl:when test="@ind2=1"><xsl:text>Publication : </xsl:text></xsl:when>
 <xsl:when test="@ind2=2"><xsl:text>Diffusion, distribution : </xsl:text></xsl:when>
 <xsl:when test="@ind2=3"><xsl:text>Fabrication : </xsl:text></xsl:when>
 <xsl:when test="@ind2=4"><xsl:text>Date de l'avis de droit d'auteur  : </xsl:text></xsl:when>
 <xsl:otherwise><xsl:text>Éditeur(s) : </xsl:text></xsl:otherwise>
 </xsl:choose>
 <xsl:if test="marc:subfield[@code='a' or @code='c']">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">3abcdefg</xsl:with-param>
 </xsl:call-template>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text/></xsl:when><xsl:otherwise><xsl:text> </xsl:text></xsl:otherwise></xsl:choose>
 </xsl:if>
 </span>
 </xsl:for-each>

 </xsl:if>
 </xsl:when>
 <xsl:otherwise>
 <!-- Publisher Statement: Alternate Graphic Representation (MARC 880) -->
 
 
 
 <xsl:if test="$display880">
 <xsl:call-template name="m880Select">
 <xsl:with-param name="basetags">260</xsl:with-param>
 <xsl:with-param name="codes">3abcdeg</xsl:with-param>
 <xsl:with-param name="class">results_summary publisher</xsl:with-param>
 <xsl:with-param name="label"> Éditeur(s) : </xsl:with-param>
 </xsl:call-template>
 </xsl:if>
 <xsl:if test="marc:datafield[@tag=260]">
 <span class="results_summary publisher"><span class="label"> Éditeur(s) : </span>
 <xsl:for-each select="marc:datafield[@tag=260]">
 <xsl:if test="marc:subfield[@code='a' or @code='c']">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">3abcdeg</xsl:with-param>
 </xsl:call-template>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text/></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </xsl:if>
 </xsl:for-each>
 </span>
 </xsl:if>
 </xsl:otherwise>
 </xsl:choose>

 <!-- Edition Statement: Alternate Graphic Representation (MARC 880) -->
 <xsl:if test="$display880">
 <xsl:call-template name="m880Select">
 <xsl:with-param name="basetags">250</xsl:with-param>
 <xsl:with-param name="codes">ab</xsl:with-param>
 <xsl:with-param name="class">results_summary edition</xsl:with-param>
 <xsl:with-param name="label">Édition&nbsp;:</xsl:with-param>
 </xsl:call-template>
 </xsl:if>
 
 <xsl:if test="marc:datafield[@tag=250]">
 <span class="results_summary edition"><span class="label">Édition&nbsp;:</span>
 <xsl:for-each select="marc:datafield[@tag=250]">
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">ab</xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text>.</xsl:text></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </xsl:for-each>
 </span>
 </xsl:if>
 
<!-- 257 -->
 <xsl:if test="$display880">
 <xsl:call-template name="m880Select">
 <xsl:with-param name="basetags">257</xsl:with-param>
 <xsl:with-param name="codes">3abcdefghijklmnopqrstuvwxyz</xsl:with-param>
 <xsl:with-param name="class">results_summary edition</xsl:with-param>
 <xsl:with-param name="label">Pays de l'agence de production : </xsl:with-param>
 </xsl:call-template>
 </xsl:if>

 <xsl:if test="marc:datafield[@tag=257]">
 <span class="results_summary edition"><span class="label">Pays de l'agence de production : </span>
 <xsl:for-each select="marc:datafield[@tag=257]">
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">3abcdefghijklmnopqrstuvwxyz</xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text/></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </xsl:for-each>
 </span>
 </xsl:if>

 <!-- Description: Alternate Graphic Representation (MARC 880) -->
 <xsl:if test="$display880">
 <xsl:call-template name="m880Select">
 <xsl:with-param name="basetags">300</xsl:with-param>
 <xsl:with-param name="codes">abceg</xsl:with-param>
 <xsl:with-param name="class">results_summary description</xsl:with-param>
 <xsl:with-param name="label">Description : </xsl:with-param>
 </xsl:call-template>
 </xsl:if>
 
 <xsl:if test="marc:datafield[@tag=300]">
 <span class="results_summary description"><span class="label">Description : </span>
 <xsl:for-each select="marc:datafield[@tag=300]">
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">abceg</xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text>.</xsl:text></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </xsl:for-each>
 </span>
 </xsl:if>
 
  <!-- 310 Current publication frequency -->
 <xsl:if test="marc:datafield[@tag=310]">
 <span class="results_summary holdings_note"><span class="label">Périodicité courante : </span>
 <xsl:for-each select="marc:datafield[@tag=310]">
 <xsl:value-of select="marc:subfield[@code='a']"/>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text>.</xsl:text></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </xsl:for-each>
 </span>
 </xsl:if>
 
  <!--321  -->
 
 <xsl:if test="marc:datafield[@tag=321]">
 <span class="results_summary holdings_note"><span class="label">Périodicité antérieure : </span>
 <xsl:for-each select="marc:datafield[@tag=321]">
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">abcdefghijklmnopqrstuvwxyz</xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text></xsl:text></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </xsl:for-each>
 </span>
 </xsl:if>
 
   <!--340-->
  <xsl:if test="marc:datafield[@tag=340]">
 <span class="results_summary description"><span class="label">Support matériel : </span>
 <xsl:for-each select="marc:datafield[@tag=340]">
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">abcdefghijklmnopqrstuvwxyz</xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text>.</xsl:text></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </xsl:for-each>
 </span>
 </xsl:if>
 
  <!--346-->
  <xsl:if test="marc:datafield[@tag=346]">
 <span class="results_summary translated_title"><span class="label">Caractéristiques vidéos : </span>
 <xsl:for-each select="marc:datafield[@tag=346]">
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">abcdefghijklmnopqrstuvwxyz</xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text></xsl:text></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </xsl:for-each>
 </span>
 </xsl:if>
 
   <!-- 347 -->
 <xsl:if test="marc:datafield[@tag=347]">
 <span class="results_summary holdings_note"><span class="label">Caractéristiques de fichier numérique : </span>
 <xsl:for-each select="marc:datafield[@tag=347]">
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">abcde</xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text></xsl:text></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </xsl:for-each>
 </span>
 </xsl:if>
 
 <!-- 362 Dates of publication -->
 <xsl:if test="marc:datafield[@tag=362]">
 <span class="results_summary holdings_note"><span class="label">Date(s) de publication(s) : </span>
 <xsl:for-each select="marc:datafield[@tag=362]">
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">a</xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text>.</xsl:text></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </xsl:for-each>
 </span>
 </xsl:if>

 <!--ISBN -->
 <xsl:if test="marc:datafield[@tag=020]">
 <span class="results_summary publisher"><span class="label"> ISBN(s) : </span>
 <xsl:for-each select="marc:datafield[@tag=020]">
 <xsl:if test="marc:subfield[@code='a']">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">a</xsl:with-param>
 </xsl:call-template>
 </xsl:if>
 <xsl:if test="marc:subfield[@code='z']">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">z</xsl:with-param>
 </xsl:call-template>
 </xsl:if>
 <xsl:if test="marc:subfield[@code='q']"><xsl:text> (</xsl:text>
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">q</xsl:with-param>
 </xsl:call-template>
 <xsl:text>)</xsl:text>
 </xsl:if>
 <xsl:text> </xsl:text>
 <xsl:text> </xsl:text>
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">g</xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text></xsl:text></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </xsl:for-each>
 </span>
 </xsl:if>

<!--022-->
 <xsl:if test="marc:datafield[@tag=022]">
 <span class="results_summary publisher"><span class="label"> ISSN(s) : </span>
 <xsl:for-each select="marc:datafield[@tag=022]">
 <xsl:if test="marc:subfield[@code='a']">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">a</xsl:with-param>
 </xsl:call-template>
 </xsl:if>
 <xsl:if test="marc:subfield[@code='z']">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">z</xsl:with-param>
 </xsl:call-template>
 </xsl:if>
 <xsl:if test="marc:subfield[@code='q']"><xsl:text> (</xsl:text>
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">q</xsl:with-param>
 </xsl:call-template>
 <xsl:text>)</xsl:text>
 </xsl:if>
 <xsl:text> </xsl:text>
 <xsl:text> </xsl:text>
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">g</xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text></xsl:text></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </xsl:for-each>
 </span>
 </xsl:if>

 <xsl:if test="marc:datafield[@tag=013]">
 <span class="results_summary patent_info">
 <span class="label">Brevet : </span>
 <xsl:for-each select="marc:datafield[@tag=013]">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">acdef</xsl:with-param>
 <xsl:with-param name="delimeter">, </xsl:with-param>
 </xsl:call-template>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text></xsl:text></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </xsl:for-each>
 </span>
 </xsl:if>

 <xsl:if test="marc:datafield[@tag=088]">
 <span class="results_summary report_number">
 <span class="label">Rapport numéro :</span>
 <xsl:for-each select="marc:datafield[@tag=088]">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">a</xsl:with-param>
 </xsl:call-template>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text></xsl:text></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </xsl:for-each>
 </span>
 </xsl:if>

 <!-- Other Title Statement: Alternate Graphic Representation (MARC 880) -->
 <xsl:if test="$display880">
 <xsl:call-template name="m880Select">
 <xsl:with-param name="basetags">246</xsl:with-param>
 <xsl:with-param name="codes">abhfgnp</xsl:with-param>
 <xsl:with-param name="class">results_summary other_title</xsl:with-param>
 <xsl:with-param name="label">Autre titre: </xsl:with-param>
 </xsl:call-template>
 </xsl:if>

 <xsl:if test="marc:datafield[@tag=246]">
 <span class="results_summary other_title"><span class="label">Autre titre: </span>
 <xsl:for-each select="marc:datafield[@tag=246]">
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">iabhfgnp</xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text>.</xsl:text></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </xsl:for-each>
 </span>
 </xsl:if>

 <xsl:if test="marc:datafield[@tag=242]">
 <span class="results_summary translated_title"><span class="label">Titre traduit :</span>
 <xsl:for-each select="marc:datafield[@tag=242]">
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">abchnp</xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text>.</xsl:text></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </xsl:for-each>
 </span>
 </xsl:if>

 <!-- Uniform Title Statement: Alternate Graphic Representation (MARC 880) 
 <xsl:if test="$display880">
 <xsl:call-template name="m880Select">
 <xsl:with-param name="basetags">130,240</xsl:with-param>
 <xsl:with-param name="codes">adfklmor</xsl:with-param>
 <xsl:with-param name="class">results_summary uniform_title</xsl:with-param>
 <xsl:with-param name="label">Titre uniforme :</xsl:with-param>
 </xsl:call-template>
 </xsl:if>

 <xsl:if test="marc:datafield[@tag=130]|marc:datafield[@tag=240]">
 <span class="results_summary uniform_title"><span class="label">Titre uniforme&nbsp;:</span>
 <xsl:for-each select="marc:datafield[@tag=130]|marc:datafield[@tag=240]">
 <xsl:variable name="str">
 <xsl:for-each select="marc:subfield">
 <xsl:if test="(contains('adfklmor',@code) and (not(../marc:subfield[@code='n' or @code='p']) or (following-sibling::marc:subfield[@code='n' or @code='p'])))">
 <xsl:value-of select="text()"/>
 <xsl:text> </xsl:text>
 </xsl:if>
 </xsl:for-each>
 </xsl:variable>
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:value-of select="substring($str,1,string-length($str)-1)"/>
 
 </xsl:with-param>
 </xsl:call-template>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text>.</xsl:text></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </xsl:for-each>
 </span>
 </xsl:if>-->
 
 <!--130-->
 <xsl:if test="marc:datafield[@tag=130]">
 <span class="results_summary uniform_titles"><span class="label">Titre uniforme : </span>
 <xsl:for-each select="marc:datafield[@tag=130]">
 <xsl:if test="marc:subfield[@code='i']">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">i</xsl:with-param>
 </xsl:call-template>
 </xsl:if>
  <a>
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=an:"<xsl:value-of select="marc:subfield[@code='9']"/>"</xsl:attribute>
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">abcdefghijklmnopqrstuvwxyztxv</xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>
 </a>
 </xsl:for-each>
 </span>
 </xsl:if>
 
   <!--240-->
  <xsl:if test="marc:datafield[@tag=240]">
 <span class="results_summary uniform_title"><span class="label">Titre(s) uniforme(s) : </span>
 <xsl:for-each select="marc:datafield[@tag=240]">
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">abcdefghijklmnopqrstuvwxyz</xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text>.</xsl:text></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </xsl:for-each>
 </span>
 </xsl:if>
 
 <!--730-->
 
  <xsl:if test="marc:datafield[@tag=730]">
 <span class="results_summary uniform_titles"><span class="label">Titre uniforme : </span>
 <xsl:for-each select="marc:datafield[@tag=730]">
 <xsl:if test="marc:subfield[@code='i']">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">i</xsl:with-param>
 </xsl:call-template>
 </xsl:if>
  <a><xsl:choose>
 <xsl:when test="$UseControlNumber = '1' and marc:subfield[@code='w']">
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=sn:<xsl:call-template name="extractControlNumber"><xsl:with-param name="subfieldW" select="marc:subfield[@code='w']"/></xsl:call-template></xsl:attribute>
 </xsl:when>
 <xsl:otherwise>
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=ti:<xsl:value-of select="marc:subfield[@code='a']"/></xsl:attribute>
 </xsl:otherwise>
 </xsl:choose>
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">abcdefghkrstuxz</xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>
 </a>
 </xsl:for-each>
 </span>
 </xsl:if>
 
 <!--511-->
<xsl:for-each select="marc:datafield[@tag=511]">
 <span class="results_summary holdings_note"><span class="label">
 <xsl:choose>
 <xsl:when test="@ind1=0"><xsl:text></xsl:text></xsl:when>
 <xsl:when test="@ind1=1"><xsl:text>Distribution : </xsl:text></xsl:when>
 <xsl:otherwise><xsl:text>Distribution : </xsl:text></xsl:otherwise>
 </xsl:choose>
 </span>
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">a</xsl:with-param>
 </xsl:call-template>
 </span>
 </xsl:for-each>
 
 <!--520-->
  <xsl:for-each select="marc:datafield[@tag=520]">
 <span class="results_summary summary"><span class="label">
 <xsl:choose>
 <xsl:when test="@ind1=0"><xsl:text>Matière : </xsl:text></xsl:when>
 <xsl:when test="@ind1=1"><xsl:text>Critique : </xsl:text></xsl:when>
 <xsl:when test="@ind1=2"><xsl:text>Portée et contenu : </xsl:text></xsl:when>
 <xsl:when test="@ind1=3"><xsl:text>Résumé analytique : </xsl:text></xsl:when>
 <xsl:when test="@ind1=4"><xsl:text>Avis sur le contenu : </xsl:text></xsl:when>
 <xsl:otherwise><xsl:text>Résumé : </xsl:text></xsl:otherwise>
 </xsl:choose>
 </span>
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">3abcu</xsl:with-param>
 </xsl:call-template>
 </span>
 </xsl:for-each>
 
  <!--521-->
 <xsl:for-each select="marc:datafield[@tag=521]">
 <span class="results_summary summary"><span class="label">
 <xsl:choose>
 <xsl:when test="@ind1=0"><xsl:text>Niveau de lecture selon l'année scolaire : </xsl:text></xsl:when>
 <xsl:when test="@ind1=1"><xsl:text>Niveau d'intérêt selon l'âge : </xsl:text></xsl:when>
 <xsl:when test="@ind1=2"><xsl:text>Niveau d'intérêt selon l'année scolaire : </xsl:text></xsl:when>
 <xsl:when test="@ind1=3"><xsl:text>Caractéristiques spéciales du public cible : </xsl:text></xsl:when>
 <xsl:when test="@ind1=4"><xsl:text>Niveau de motivation/d'intérêt  : </xsl:text></xsl:when>
 <xsl:when test="@ind1=8"><xsl:text> </xsl:text></xsl:when>
 <xsl:otherwise><xsl:text>Public cible : </xsl:text></xsl:otherwise>
 </xsl:choose>
 </span>
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">3abcu</xsl:with-param>
 </xsl:call-template>
 </span>
 </xsl:for-each>
 
  <!--546-->
<xsl:for-each select="marc:datafield[@tag=546]">
 <span class="results_summary holdings_note"><span class="label">
 <xsl:choose>
 <xsl:when test="@ind1=0"><xsl:text></xsl:text></xsl:when>
 <xsl:when test="@ind1=1"><xsl:text> : </xsl:text></xsl:when>
 <xsl:otherwise><xsl:text>Note sur les langues : </xsl:text></xsl:otherwise>
 </xsl:choose>
 </span>
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">3abz</xsl:with-param>
 </xsl:call-template>
 </span>
 </xsl:for-each>

 <!-- sujets -->
 <xsl:if test="marc:datafield[substring(@tag, 1, 1) = '6']">
 
 <h6><span class="label">Sujet(s)&nbsp;: </span>
 <div class='contentblock'>
 <xsl:for-each select="marc:datafield[substring(@tag, 1, 1) = '6']">
 <a>
 <xsl:choose>
 <xsl:when test="marc:subfield[@code=9] and $UseAuthoritiesForTracings='1'">
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=an:<xsl:value-of select="marc:subfield[@code=9]"/></xsl:attribute>
 </xsl:when>
 <!-- #1807 Strip unwanted parenthesis from subjects for searching -->
            <xsl:when test="$TraceSubjectSubdivisions='1'">
                <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=<xsl:call-template name="subfieldSelectSubject">
                        <xsl:with-param name="codes">abcdfgklmnopqrstvxyz</xsl:with-param>
                        <xsl:with-param name="delimeter"> AND </xsl:with-param>
                        <xsl:with-param name="prefix">(su<xsl:value-of select="$SubjectModifier"/>:<xsl:value-of select="$TracingQuotesLeft"/></xsl:with-param>
                        <xsl:with-param name="suffix"><xsl:value-of select="$TracingQuotesRight"/>)</xsl:with-param>
                    </xsl:call-template>
                </xsl:attribute>
            </xsl:when>
                <!-- #1807 Strip unwanted parenthesis from subjects for searching -->

 <xsl:otherwise>
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=su<xsl:value-of select="$SubjectModifier"/><xsl:value-of select="$TracingQuotesLeft"/><xsl:value-of select="marc:subfield[@code='a']"/><xsl:value-of select="$TracingQuotesRight"/></xsl:attribute>
 </xsl:otherwise>
 </xsl:choose>
 
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">abcdfgklmnopqrstvxyz</xsl:with-param>
 <xsl:with-param name="subdivCodes">vxyz</xsl:with-param>
 <xsl:with-param name="subdivDelimiter">-- </xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>

 </a>
 <xsl:if test="marc:subfield[@code=9]">
 <a class='authlink'>
 <xsl:attribute name="href">/cgi-bin/koha/authorities/detail.pl?authid=<xsl:value-of select="marc:subfield[@code=9]"/></xsl:attribute>
  <xsl:text>  </xsl:text>
  <img style="vertical-align:middle" height="15" width="15" src="/opac-tmpl/bootstrap/images/filefind.png"/>
 </a>
 </xsl:if>
 <xsl:choose>
 <xsl:when test="position()=last()"></xsl:when>
 <xsl:otherwise><br /></xsl:otherwise>
 </xsl:choose>

 </xsl:for-each>
 </div>
 </h6>
 </xsl:if>

 <xsl:if test="marc:datafield[@tag=856]">
 <span class="results_summary online_resources"><span class="label">Ressources en ligne&nbsp;:</span>
 <xsl:for-each select="marc:datafield[@tag=856]">
 <xsl:variable name="SubqText"><xsl:value-of select="marc:subfield[@code='q']"/></xsl:variable>
 <a><xsl:attribute name="href"><xsl:value-of select="marc:subfield[@code='u']"/></xsl:attribute>
 <xsl:choose>
 <xsl:when test="($Show856uAsImage='Details' or $Show856uAsImage='Both') and (substring($SubqText,1,6)='image/' or $SubqText='img' or $SubqText='bmp' or $SubqText='cod' or $SubqText='gif' or $SubqText='ief' or $SubqText='jpe' or $SubqText='jpeg' or $SubqText='jpg' or $SubqText='jfif' or $SubqText='png' or $SubqText='svg' or $SubqText='tif' or $SubqText='tiff' or $SubqText='ras' or $SubqText='cmx' or $SubqText='ico' or $SubqText='pnm' or $SubqText='pbm' or $SubqText='pgm' or $SubqText='ppm' or $SubqText='rgb' or $SubqText='xbm' or $SubqText='xpm' or $SubqText='xwd')">
 <xsl:element name="img"><xsl:attribute name="src"><xsl:value-of select="marc:subfield[@code='u']"/></xsl:attribute><xsl:attribute name="alt"><xsl:value-of select="marc:subfield[@code='y']"/></xsl:attribute><xsl:attribute name="height">100</xsl:attribute></xsl:element><xsl:text></xsl:text>
 </xsl:when>
 <xsl:when test="marc:subfield[@code='y' or @code='3' or @code='z']">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">y3z</xsl:with-param>
 </xsl:call-template>
 </xsl:when>
 <xsl:when test="not(marc:subfield[@code='y']) and not(marc:subfield[@code='3']) and not(marc:subfield[@code='z'])">
 <xsl:choose>
 <xsl:when test="$URLLinkText!=''">
 <xsl:value-of select="$URLLinkText"/>
 </xsl:when>
 <xsl:otherwise>
 <xsl:text>Cliquez ici pour consulter en ligne</xsl:text>
 </xsl:otherwise>
 </xsl:choose>
 </xsl:when>
 </xsl:choose>
 </a>
 <xsl:choose>
 <xsl:when test="position()=last()"><xsl:text> </xsl:text></xsl:when>
 <xsl:otherwise> | </xsl:otherwise>
 </xsl:choose>

 </xsl:for-each>
 </span>
 </xsl:if>
 <xsl:if test="marc:datafield[@tag=505]">
 <div class="results_summary contents">
 <xsl:choose>
 <xsl:when test="marc:datafield[@tag=505]/@ind1=0">
 <span class="label">Dépouillement complet : </span>
 </xsl:when>
 <xsl:when test="marc:datafield[@tag=505]/@ind1=1">
 <span class="label">Dépouillement incomplet : </span>
 </xsl:when>
 <xsl:when test="marc:datafield[@tag=505]/@ind1=2">
 <span class="label">Dépouillement partiel : </span>
 </xsl:when>
 </xsl:choose>
 <xsl:for-each select="marc:datafield[@tag=505]">
 <div class='contentblock'>
 <xsl:choose>
 <xsl:when test="@ind2=0">
 <xsl:call-template name="subfieldSelectSpan">
 <xsl:with-param name="codes">tru</xsl:with-param>
 </xsl:call-template>
 </xsl:when>
 <xsl:otherwise>
 <xsl:call-template name="subfieldSelectSpan">
 <xsl:with-param name="codes">atru</xsl:with-param>
 </xsl:call-template>
 </xsl:otherwise>
 </xsl:choose>
 </div>
 </xsl:for-each>
 </div>
 </xsl:if>

 
 <!-- 773 -->
 <xsl:if test="marc:datafield[@tag=773]">
 <xsl:for-each select="marc:datafield[@tag=773]">
 <xsl:if test="@ind1=0">
 <span class="results_summary in"><span class="label">
 <xsl:choose>
 <xsl:when test="@ind2=' '">
 Dans : </xsl:when>
 <xsl:when test="@ind2=8">
 <xsl:if test="marc:subfield[@code='i']">
 <xsl:value-of select="marc:subfield[@code='i']"/>
 </xsl:if>
 </xsl:when>
 </xsl:choose>
 </span>
 <xsl:variable name="f773">
 <xsl:call-template name="chopPunctuation"><xsl:with-param name="chopString"><xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">a_t</xsl:with-param>
 </xsl:call-template></xsl:with-param></xsl:call-template>
 </xsl:variable>
 <xsl:choose>
 <xsl:when test="$UseControlNumber = '1' and marc:subfield[@code='w']">
 <a><xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=sn:<xsl:call-template name="extractControlNumber"><xsl:with-param name="subfieldW" select="marc:subfield[@code='w']"/></xsl:call-template></xsl:attribute>
 <xsl:value-of select="translate($f773, '()', '')"/>
 </a>
 <xsl:if test="marc:subfield[@code='g']"><xsl:text> </xsl:text><xsl:value-of select="marc:subfield[@code='g']"/></xsl:if>
 </xsl:when>
 <xsl:when test="marc:subfield[@code='0']">
 <a><xsl:attribute name="href">/cgi-bin/koha/catalogue/detail.pl?biblionumber=<xsl:value-of select="marc:subfield[@code='0']"/></xsl:attribute>
 <xsl:value-of select="$f773"/>
 </a>
 </xsl:when>
 <xsl:otherwise>
 <a><xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=ti,phr:<xsl:value-of select="translate($f773, '()', '')"/></xsl:attribute>
 <xsl:value-of select="$f773"/>
 </a>
 <xsl:if test="marc:subfield[@code='g']"><xsl:text> </xsl:text><xsl:value-of select="marc:subfield[@code='g']"/></xsl:if>
 <xsl:if test="marc:subfield[@code='m']"><xsl:text> </xsl:text><xsl:value-of select="marc:subfield[@code='m']"/></xsl:if>
<xsl:if test="marc:subfield[@code='d']"><xsl:text> </xsl:text><xsl:value-of select="marc:subfield[@code='d']"/></xsl:if>
<xsl:if test="marc:subfield[@code='h']"><xsl:text> </xsl:text><xsl:value-of select="marc:subfield[@code='h']"/></xsl:if>
 </xsl:otherwise>
 </xsl:choose>
 </span>
 <xsl:if test="marc:subfield[@code='n']">
 <span class="results_summary"><xsl:value-of select="marc:subfield[@code='n']"/></span>
 </xsl:if>

 </xsl:if>
 </xsl:for-each>
 </xsl:if>
<!--502-->
 <xsl:if test="marc:datafield[@tag=502]">
 <span class="results_summary diss_note">
 <span class="label">Note de thèse : </span>
 <xsl:for-each select="marc:datafield[@tag=502]">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">abcdgo</xsl:with-param>
 </xsl:call-template>
 </xsl:for-each>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text></xsl:text></xsl:when><xsl:otherwise><xsl:text> </xsl:text></xsl:otherwise></xsl:choose>
 </span>
 </xsl:if>

 <!-- 866 textual holdings -->
 <xsl:if test="marc:datafield[@tag=866]">
 <span class="results_summary holdings_note"><span class="label">Note d'exemplaire :</span>
 <xsl:for-each select="marc:datafield[@tag=866]">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">axz</xsl:with-param>
 </xsl:call-template>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text></xsl:text></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </xsl:for-each>
 </span>
 </xsl:if>
 
 <!-- Preceding Title Statement: Alternate Graphic Representation (MARC 765) -->
 <xsl:if test="$display880">
 <xsl:call-template name="m880Select">
 <xsl:with-param name="basetags">765</xsl:with-param>
 <xsl:with-param name="codes">abcdefghikrstuxz</xsl:with-param>
 <xsl:with-param name="class">results_summary preceding_title</xsl:with-param>
 <xsl:with-param name="label">Liaison(s) : </xsl:with-param>
 </xsl:call-template>
 </xsl:if>

 <xsl:if test="marc:datafield[@tag=765]">
 <xsl:for-each select="marc:datafield[@tag=765]">
 <xsl:if test="@ind1=' ' or @ind1=0 or @ind1=1">
 <span class="results_summary edition">
 <xsl:choose>
 <xsl:when test="@ind2=' '">
 <span class="label">Traduction de : </span>
 </xsl:when>
 <xsl:when test="@ind2=8">
 <span class="label"></span>
 </xsl:when>
 </xsl:choose>
 <div class="contentblock">
 <a>
 <xsl:choose>
  <xsl:when test="$UseControlNumber = '1' and marc:subfield[@code='w']">
                <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=sn:<xsl:call-template name="extractControlNumber"><xsl:with-param name="subfieldW" select="marc:subfield[@code='w']"/></xsl:call-template></xsl:attribute>
            </xsl:when>
            <xsl:otherwise>
                <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=<xsl:value-of select="marc:subfield[@code='t']"/></xsl:attribute>
            </xsl:otherwise>
 </xsl:choose>
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">abcdefghijklmnopqrstuvxy</xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>
 </a>
 </div>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text></xsl:text></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </span>
  </xsl:if>
 </xsl:for-each>
 </xsl:if>
 
<!-- Preceding Title Statement: Alternate Graphic Representation (MARC 880) -->
<xsl:if test="marc:datafield[@tag=770]">
 <xsl:for-each select="marc:datafield[@tag=770]">
 <xsl:if test="@ind1=' ' or @ind1=0 or @ind1=1">
 <span class="results_summary edition">
 <xsl:choose>
 <xsl:when test="@ind2=' '">
 <span class="label">Supplément : </span>
 </xsl:when>
 <xsl:when test="@ind2=8">
 <span class="label"></span>
 </xsl:when>
 </xsl:choose>
 <a>
 <xsl:choose>
 <xsl:when test="$UseControlNumber = '1' and marc:subfield[@code='w']">
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=sn:<xsl:call-template name="extractControlNumber"><xsl:with-param name="subfieldW" select="marc:subfield[@code='w']"/></xsl:call-template></xsl:attribute>
 </xsl:when>
 <xsl:otherwise>
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=<xsl:value-of select="marc:subfield[@code='t']"/></xsl:attribute>
 </xsl:otherwise>
 </xsl:choose>
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">abcdefghikrstuxz</xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>
 </a>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text></xsl:text></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </span>
  </xsl:if>
 </xsl:for-each>
 </xsl:if>
 
 <!-- 772-->
 <xsl:if test="$display880">
 <xsl:call-template name="m880Select">
 <xsl:with-param name="basetags">772</xsl:with-param>
 <xsl:with-param name="codes">abcdefghikrstuxz</xsl:with-param>
 <xsl:with-param name="class">results_summary preceding_title</xsl:with-param>
 <xsl:with-param name="label">Supplément à : </xsl:with-param>
 </xsl:call-template>
 </xsl:if>

 <xsl:if test="marc:datafield[@tag=772]">
 <xsl:for-each select="marc:datafield[@tag=772]">
 <xsl:if test="@ind1=' ' or @ind1=0 or @ind1=1">
 <span class="results_summary edition">
 <xsl:choose>
 <xsl:when test="@ind2=' '">
 <span class="label">Supplément à : </span>
 </xsl:when>
 <xsl:when test="@ind2=0">
 <span class="label">Parent : </span>
 </xsl:when>
 <xsl:when test="@ind2=8">
 <span class="label"></span>
 </xsl:when>
 </xsl:choose>
 <a>
 <xsl:choose>
 <xsl:when test="$UseControlNumber = '1' and marc:subfield[@code='w']">
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=sn:<xsl:call-template name="extractControlNumber"><xsl:with-param name="subfieldW" select="marc:subfield[@code='w']"/></xsl:call-template></xsl:attribute>
 </xsl:when>
 <xsl:otherwise>
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=ti,wrdl:<xsl:value-of select="marc:subfield[@code='t']"/></xsl:attribute>
 </xsl:otherwise>
 </xsl:choose>
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">abcdefghikrstuxz</xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>
 </a>
 <xsl:choose><xsl:when test="position()=last()"><xsl:text></xsl:text></xsl:when><xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise></xsl:choose>
 </span>
 </xsl:if>
 </xsl:for-each>
 </xsl:if>
 
 <!--  775 Other Edition  -->
        <xsl:if test="marc:datafield[@tag=775]">
		<xsl:for-each select="marc:datafield[@tag=775]">
		<xsl:if test="@ind1=0 or @ind1=' ' or @ind1=1">
        <span class="results_summary other_editions">
 <xsl:choose>
 <xsl:when test="@ind2=' '">
 <span class="label">Autre(s) édition(s) disponible(s) : </span>
 </xsl:when>
 <xsl:otherwise><span class="label"></span></xsl:otherwise>
  </xsl:choose>
       
            <xsl:variable name="f775">
                <xsl:call-template name="chopPunctuation"><xsl:with-param name="chopString"><xsl:call-template name="subfieldSelect">
                <xsl:with-param name="codes">abcdefghjklmnopqrstuvxy</xsl:with-param>
                </xsl:call-template></xsl:with-param></xsl:call-template>
            </xsl:variable>
            <xsl:if test="marc:subfield[@code='i']">
                <xsl:call-template name="subfieldSelect">
                    <xsl:with-param name="codes">i</xsl:with-param>
                </xsl:call-template>
                <xsl:text> </xsl:text>
            </xsl:if>
 <a>
 <xsl:choose>
 <xsl:when test="$UseControlNumber = '1' and marc:subfield[@code='w']">
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=sn:<xsl:call-template name="extractControlNumber"><xsl:with-param name="subfieldW" select="marc:subfield[@code='w']"/></xsl:call-template></xsl:attribute>
 </xsl:when>
 <xsl:otherwise>
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=ti,phr:<xsl:value-of select="translate($f775, '()', '')"/></xsl:attribute>
 </xsl:otherwise>
 </xsl:choose>
 <xsl:call-template name="subfieldSelect">
                <xsl:with-param name="codes">abcdefghjklmnopqrstuvxy</xsl:with-param>
            </xsl:call-template>
 </a>
 <xsl:choose>
 <xsl:when test="position()=last()"></xsl:when>
 <xsl:otherwise><xsl:text>; </xsl:text></xsl:otherwise>
 </xsl:choose>
  </span>
  </xsl:if>
 </xsl:for-each>
 </xsl:if>
 
 <!--776-->
  <xsl:if test="marc:datafield[@tag=776]">
 <span class="results_summary edition"><span class="label">Liaison(s) : </span>
 <xsl:for-each select="marc:datafield[@tag=776]">

 <div class="contentblock">
 <a>
 <xsl:choose>
            <xsl:when test="$UseControlNumber = '1' and marc:subfield[@code='w']">
                <xsl:attribute name="href">search.pl?q=sn:<xsl:call-template name="extractControlNumber"><xsl:with-param name="subfieldW" select="marc:subfield[@code='w']"/></xsl:call-template></xsl:attribute>
            </xsl:when>
            <xsl:otherwise>
                <xsl:attribute name="href">search.pl?q=ti:<xsl:value-of select="marc:subfield[@code='t']"/> and <xsl:value-of select="marc:subfield[@code='a']"/></xsl:attribute>
            </xsl:otherwise>
            </xsl:choose>
            <xsl:call-template name="subfieldSelect">
                <xsl:with-param name="codes">abcdefghjklmnopqrstuvxyz</xsl:with-param>
            </xsl:call-template>
 </a>
 </div>
 </xsl:for-each>
 </span>
 </xsl:if>
 
 <!-- 777-->
 <xsl:if test="marc:datafield[@tag=777]">
 <span class="results_summary edition"><span class="label">Liaison(s) : </span>
 <xsl:for-each select="marc:datafield[@tag=777]">
 <a>
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=ti:<xsl:value-of select="marc:subfield[@code='t']"/>"</xsl:attribute>
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">abditz</xsl:with-param>
 </xsl:call-template>
 </xsl:with-param>
 </xsl:call-template>
 </a>
 </xsl:for-each>
 </span>
 </xsl:if>
 

 <!-- 780 -->
 <xsl:if test="marc:datafield[@tag=780]">
 <xsl:for-each select="marc:datafield[@tag=780]">
 <xsl:if test="@ind1=0 or @ind1=' '">
 <span class="results_summary preceeding_entry">
 <xsl:choose>
 <xsl:when test="@ind2=0">
 <span class="label">Fait suite à : </span>
 </xsl:when>
 <xsl:when test="@ind2=1">
 <span class="label">Fait suite après scission de : </span>
 </xsl:when>
 <xsl:when test="@ind2=2">
 <span class="label">Remplace : </span>
 </xsl:when>
 <xsl:when test="@ind2=3">
 <span class="label">Remplace en partie : </span>
 </xsl:when>
 <xsl:when test="@ind2=4">
 <span class="label">Fusion de ... et de ... </span>
 </xsl:when>
 <xsl:when test="@ind2=5">
 <span class="label">A absorbé : </span>
 </xsl:when>
 <xsl:when test="@ind2=6">
 <span class="label">A absorbé en partie : </span>
 </xsl:when>
 <xsl:when test="@ind2=7">
 <span class="label">Scission de: </span>
 </xsl:when>
 </xsl:choose>
<a>
 <xsl:choose>
 <xsl:when test="$UseControlNumber = '1' and marc:subfield[@code='w']">
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=sn:<xsl:call-template name="extractControlNumber"><xsl:with-param name="subfieldW" select="marc:subfield[@code='w']"/></xsl:call-template></xsl:attribute>
 </xsl:when>
 <xsl:otherwise>
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=<xsl:value-of select="marc:subfield[@code='t' or @code='a']"/></xsl:attribute>
 </xsl:otherwise>
 </xsl:choose>
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">abcdefghijklmnopqrstuvxyz</xsl:with-param>
 </xsl:call-template>
 </a>
 </span>

 <xsl:if test="marc:subfield[@code='n']">
 <span class="results_summary"><xsl:value-of select="marc:subfield[@code='n']"/></span>
 </xsl:if>

 </xsl:if>
 </xsl:for-each>
 </xsl:if>

 <!-- 785 -->
 <xsl:if test="marc:datafield[@tag=785]">
 <xsl:for-each select="marc:datafield[@tag=785]">

 <xsl:if test="@ind1=0 or @ind1=' '">
 <span class="results_summary succeeding_entry">
 <xsl:choose>
 <xsl:when test="@ind2=0">
 <span class="label">Suivi de : </span>
 </xsl:when>
 <xsl:when test="@ind2=1">
 <span class="label">Suivi en partie de : </span>
 </xsl:when>
 <xsl:when test="@ind2=2">
 <span class="label">Remplacé par : </span>
 </xsl:when>
 <xsl:when test="@ind2=3">
 <span class="label">Remplacé en partie par : </span>
 </xsl:when>
 <xsl:when test="@ind2=4">
 <span class="label">Absorbé par :</span>
 </xsl:when>
 <xsl:when test="@ind2=5">
 <span class="label">Absorbé en partie par : </span>
 </xsl:when>
 <xsl:when test="@ind2=6">
 <span class="label">Scindé en ... et ... : </span>
 </xsl:when>
 <xsl:when test="@ind2=7">
 <span class="label">Fusionné avec ... et devient ...: </span>
 </xsl:when>
 <xsl:when test="@ind2=8">
 <span class="label">Redevient : </span>
 </xsl:when>

 </xsl:choose>
<a>
 <xsl:choose>
 <xsl:when test="$UseControlNumber = '1' and marc:subfield[@code='w']">
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=sn:<xsl:call-template name="extractControlNumber"><xsl:with-param name="subfieldW" select="marc:subfield[@code='w']"/></xsl:call-template></xsl:attribute>
 </xsl:when>
 <xsl:otherwise>
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=<xsl:value-of select="marc:subfield[@code='t' or @code='a']"/></xsl:attribute>
 </xsl:otherwise>
 </xsl:choose>
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">abcdefghijklmnopqrstuvxyz</xsl:with-param>
 </xsl:call-template>
 </a>
 </span>
 </xsl:if>

 </xsl:for-each>
 </xsl:if>
 
<!-- 787 -->

        <xsl:if test="marc:datafield[@tag=787]">
        <xsl:for-each select="marc:datafield[@tag=787]">
		<xsl:if test="@ind1=0 or @ind1=' '">
 <span class="results_summary in">
 <xsl:choose>
 <xsl:when test="@ind2=' '">
 <span class="label">Document associé : </span>
 </xsl:when>
 <xsl:otherwise><span class="label"></span></xsl:otherwise>
 </xsl:choose>
            <xsl:variable name="f787">
                <xsl:call-template name="chopPunctuation"><xsl:with-param name="chopString"><xsl:call-template name="subfieldSelect">
                <xsl:with-param name="codes">abcdefghjklmopqrstuvxyz</xsl:with-param>
                </xsl:call-template></xsl:with-param></xsl:call-template>
            </xsl:variable>
            <span class="label">
            <xsl:if test="marc:subfield[@code='i']">
                <xsl:call-template name="subfieldSelect">
                    <xsl:with-param name="codes">i</xsl:with-param>
                </xsl:call-template>
                <xsl:text> </xsl:text>
            </xsl:if>
            </span>
            <a>
            <xsl:choose>
            <xsl:when test="$UseControlNumber = '1' and marc:subfield[@code='w']">
                <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=sn:<xsl:call-template name="extractControlNumber"><xsl:with-param name="subfieldW" select="marc:subfield[@code='w']"/></xsl:call-template></xsl:attribute>
            </xsl:when>
            <xsl:otherwise>
                <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=<xsl:value-of select="marc:subfield[@code='t']"/></xsl:attribute>
                </xsl:otherwise>
            </xsl:choose>
            <xsl:call-template name="subfieldSelect">
                <xsl:with-param name="codes">abcdefghjklmopqrstuvxyz</xsl:with-param>
            </xsl:call-template>
            </a>
            <xsl:choose>
                <xsl:when test="position()=last()"></xsl:when>
                <xsl:otherwise><xsl:text> </xsl:text></xsl:otherwise>
            </xsl:choose>
			</span>
			</xsl:if>
        </xsl:for-each>
		</xsl:if>
 
 <!--590-->
<xsl:for-each select="marc:datafield[@tag=590]">
 <span class="results_summary holdings_note"><span class="label">
 <xsl:choose>
 <xsl:when test="@ind1=0"><xsl:text></xsl:text></xsl:when>
 <xsl:when test="@ind1=1"><xsl:text>Note locale : </xsl:text></xsl:when>
 <xsl:otherwise><xsl:text>Note locale : </xsl:text></xsl:otherwise>
 </xsl:choose>
 </span>
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">a</xsl:with-param>
 </xsl:call-template>
 </span>
 </xsl:for-each>

 <xsl:if test="$OPACBaseURL!=''">
 <span class="results_summary"><span class="label">Vue OPAC: </span>
 <a><xsl:attribute name="href"><xsl:value-of select="$OPACBaseURL"/>/cgi-bin/koha/opac-detail.pl?biblionumber=<xsl:value-of select="marc:datafield[@tag=999]/marc:subfield[@code='c']"/></xsl:attribute><xsl:attribute name="target">_blank</xsl:attribute>Ouvrir dans une nouvelle fenêtre</a>. </span>
 </xsl:if>
 
 </xsl:template>

  <xsl:template name="showAuthor">
 <xsl:param name="authorfield" />
 <xsl:param name="UseAuthoritiesForTracings" />
 <xsl:for-each select="$authorfield">
 <xsl:choose><xsl:when test="position()!=1"><xsl:text> | </xsl:text>
 </xsl:when>
 </xsl:choose>
 <xsl:choose>
 <xsl:when test="not(@tag=111 or @tag=711)" />
 <xsl:text></xsl:text>
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">andc</xsl:with-param>
 </xsl:call-template>
 <xsl:text></xsl:text>
 </xsl:choose>
 
 <xsl:if test="marc:subfield[@code='i']">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">i</xsl:with-param>
 </xsl:call-template>
 </xsl:if>
  <a>
 <xsl:choose>
 <xsl:when test="marc:subfield[@code=9] and $UseAuthoritiesForTracings='1'">
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=an:<xsl:value-of select="marc:subfield[@code=9]"/></xsl:attribute>
 </xsl:when>
 <xsl:otherwise>
 <xsl:attribute name="href">/cgi-bin/koha/catalogue/search.pl?q=au:<xsl:value-of select="marc:subfield[@code='a']"/></xsl:attribute>
 </xsl:otherwise>
 </xsl:choose>
 <xsl:choose>
 <xsl:when test="@tag=100 or @tag=700"><xsl:call-template name="nameABCDQ"/></xsl:when>
 <xsl:when test="@tag=110 or @tag=710"><xsl:call-template name="nameABCDQ"/></xsl:when>
 <xsl:when test="@tag=111 or @tag=711"><xsl:call-template name="nameACDEQ"/></xsl:when>
 </xsl:choose>

 </a>
  <xsl:text>  </xsl:text>
 <xsl:if test="marc:subfield[@code='e']">
 <xsl:call-template name="subfieldSelect">
 <xsl:with-param name="codes">e</xsl:with-param>
 </xsl:call-template>
 </xsl:if>
 <xsl:if test="marc:subfield[@code=9]">
                <a class='authlink'>
                    <xsl:attribute name="href">/cgi-bin/koha/authorities/detail.pl?authid=<xsl:value-of select="marc:subfield[@code=9]"/></xsl:attribute>
                     <xsl:text>  </xsl:text>
					<img style="vertical-align:middle" height="15" width="15" src="/opac-tmpl/bootstrap/images/filefind.png"/>
                </a>
            </xsl:if>
 </xsl:for-each>
 <xsl:text> </xsl:text>
 </xsl:template>
 
  <xsl:template name="nameABCDQ">
            <xsl:call-template name="chopPunctuation">
                <xsl:with-param name="chopString">
                    <xsl:call-template name="subfieldSelect">
                        <xsl:with-param name="codes">abcdfghjklmnopqrstuvwxyz</xsl:with-param>
                    </xsl:call-template>
                </xsl:with-param>
                <xsl:with-param name="punctuation">
                    <xsl:text>:,;/ </xsl:text>
                </xsl:with-param>
            </xsl:call-template>
        <xsl:call-template name="termsOfAddress"/>
    </xsl:template>

<xsl:template name="nameABCDN">
        <xsl:for-each select="marc:subfield[@code='a']">
                <xsl:call-template name="chopPunctuation">
                    <xsl:with-param name="chopString" select="."/>
                </xsl:call-template>
        </xsl:for-each>
        <xsl:for-each select="marc:subfield[@code='b']">
                <xsl:value-of select="."/>
        </xsl:for-each>
        <xsl:if test="marc:subfield[@code='c'] or marc:subfield[@code='d'] or marc:subfield[@code='n']">
                <xsl:call-template name="subfieldSelect">
                    <xsl:with-param name="codes">cdn</xsl:with-param>
                </xsl:call-template>
        </xsl:if>
    </xsl:template>

   <xsl:template name="nameACDEQ">
            <xsl:call-template name="subfieldSelect">
                <xsl:with-param name="codes">andcqti</xsl:with-param>
            </xsl:call-template>
    </xsl:template>
	
	<xsl:template name="termsOfAddress">
        <xsl:if test="marc:subfield[@code='b' or @code='c']">
            <xsl:call-template name="chopPunctuation">
                <xsl:with-param name="chopString">
                    <xsl:call-template name="subfieldSelect">
                        <xsl:with-param name="codes"></xsl:with-param>
                    </xsl:call-template>
                </xsl:with-param>
            </xsl:call-template>
        </xsl:if>
    </xsl:template>

<xsl:template name="part">
 <xsl:variable name="partNumber">
 <xsl:call-template name="specialSubfieldSelect">
 <xsl:with-param name="axis">n</xsl:with-param>
 <xsl:with-param name="anyCodes"></xsl:with-param>
 <xsl:with-param name="afterCodes">fghkdlmor</xsl:with-param>
 </xsl:call-template>
 </xsl:variable>
 <xsl:variable name="partName">
            <xsl:call-template name="specialSubfieldSelect">
                <xsl:with-param name="axis">p</xsl:with-param>
                <xsl:with-param name="anyCodes"></xsl:with-param>
                <xsl:with-param name="afterCodes">fghkdlmor</xsl:with-param>
            </xsl:call-template>
        </xsl:variable>
 
 <xsl:if test="string-length(normalize-space($partNumber))">
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString" select="$partNumber"/>
 </xsl:call-template>
 </xsl:if>
 <xsl:if test="string-length(normalize-space($partName))">
 <xsl:call-template name="chopPunctuation">
 <xsl:with-param name="chopString" select="$partName"/>
 </xsl:call-template>
 </xsl:if>
 </xsl:template>
 
 <xsl:variable name="partName">
 <xsl:call-template name="specialSubfieldSelect">
 <xsl:with-param name="axis"></xsl:with-param>
 <xsl:with-param name="anyCodes"></xsl:with-param>
 <xsl:with-param name="afterCodes">fghkdlmor</xsl:with-param>
 </xsl:call-template>
 </xsl:variable>
 <xsl:template name="specialSubfieldSelect">
 <xsl:param name="anyCodes"/>
 <xsl:param name="axis"/>
 <xsl:param name="beforeCodes"/>
 <xsl:param name="afterCodes"/>
 <xsl:variable name="str">
 <xsl:for-each select="marc:subfield">
 <xsl:if test="contains($anyCodes, @code)      or (contains($beforeCodes,@code) and following-sibling::marc:subfield[@code=$axis])      or (contains($afterCodes,@code) and preceding-sibling::marc:subfield[@code=$axis])">
 <xsl:value-of select="text()"/>
 <xsl:text> </xsl:text>
 </xsl:if>
 </xsl:for-each>
 </xsl:variable>
 <xsl:value-of select="substring($str,1,string-length($str)-1)"/>
 </xsl:template>

 <!-- #1807 Strip unwanted parenthesis from subjects for searching -->
    <xsl:template name="subfieldSelectSubject">
        <xsl:param name="codes"/>
        <xsl:param name="delimeter"><xsl:text> </xsl:text></xsl:param>
        <xsl:param name="subdivCodes"/>
        <xsl:param name="subdivDelimiter"/>
        <xsl:param name="prefix"/>
        <xsl:param name="suffix"/>
        <xsl:variable name="str">
            <xsl:for-each select="marc:subfield">
                <xsl:if test="contains($codes, @code)">
                    <xsl:if test="contains($subdivCodes, @code)">
                        <xsl:value-of select="$subdivDelimiter"/>
                    </xsl:if>
                    <xsl:value-of select="$prefix"/><xsl:value-of select="translate(text(),'()','')"/><xsl:value-of select="$suffix"/><xsl:value-of select="$delimeter"/>
                </xsl:if>
            </xsl:for-each>
        </xsl:variable>
        <xsl:value-of select="substring($str,1,string-length($str)-string-length($delimeter))"/>
    </xsl:template>

</xsl:stylesheet>
