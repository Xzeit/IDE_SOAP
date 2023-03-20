<script setup>
import {onMounted, reactive} from "vue";
import { XMLParser } from "fast-xml-parser";

const variable = reactive({
  response: '',
  IDEnumber: '',
})

const method = async () => {
  const response = await fetch(`https://www.uid-wse-a.admin.ch/V5.0/PublicServices.svc`, {
    method: "POST",
    body :
        '<?xml version="1.0" encoding="utf-8"?>' +
        "<soapenv:Envelope xmlns:soapenv=\"http://schemas.xmlsoap.org/soap/envelope/\" xmlns:uid=\"http://www.uid.admin.ch/xmlns/uid-wse\" xmlns:ns=\"http://www.ech.ch/xmlns/eCH-0097/5\">" +
        "<soapenv:Header/>" +
        "<soapenv:Body><uid:GetByUID><uid:uid><ns:uidOrganisationId>-"+ variable.IDEnumber +"</ns:uidOrganisationId></uid:uid></uid:GetByUID></soapenv:Body></soapenv:Envelope>",

    headers: {
      "Content-Type" : "text/xml; charset=utf-8",
      SOAPAction: "http://www.uid.admin.ch/xmlns/uid-wse/IPublicServices/GetByUID"
    }
  })
  const parser = new XMLParser();

  variable.response = parser.parse(await response.text());
};

onMounted(() => {
  method()
})
</script>

<template>
  <div class="container">
    <form>
      <label for="UID">Numero IDE</label><br/>
      <input id="UID" type="text" v-model.trim="variable.IDEnumber" :onchange="method"/>
    </form>
    {{ variable.response }}
  </div>
</template>

<style scoped>
</style>
