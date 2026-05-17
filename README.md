## n8n-Build-a-Private-AI-Email-Assistant

#### mdwasimu02@gmail.com theke mdwasimu015@gmail.com e ekta test email send koro.

#### click--->Add first step--->search: gmail--->click: On message received--->simplify off koro--->click koro: Fetch Test Even 

#### Gmail Trigger er +sign click koro--->search: HTML--->click: Extract html content---> Key te value daw: email_body--->CSS Selector e value daw: body--->JSON Property e value daw(pash theke text tene ene boshaw. text hobe oita jeta tomar email e text asche.)--->click: Execute step
 
#### HTML er +sign click koro--->search & click: ai agent--->Source for Prompt (User Message) e select koro: Define below--->Prompt (User Message) e value daw: prompt.txt theke text copy kore paste koro.--->prompt text theke remove koro: {{ $json.email_body }}--->pash theke email body tene ene boshaw.

#### Chat model er +sign click koro--->search koro & clic koro: Google Gemini Chat Model--->Credential set koro ar model select koro.

#### AI Agent er +sign click koro--->search koro & click koro: gmail--->click: Send a message--->
