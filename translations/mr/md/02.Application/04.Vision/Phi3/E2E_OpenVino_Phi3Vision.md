<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "d7d7afa242a4a041ff4193546d4baf16",
  "translation_date": "2025-05-09T19:59:19+00:00",
  "source_file": "md/02.Application/04.Vision/Phi3/E2E_OpenVino_Phi3Vision.md",
  "language_code": "mr"
}
-->
हा डेमो एका pretrained मॉडेलचा वापर करून एखाद्या प्रतिमा आणि टेक्स्ट प्रॉम्प्टवरून Python कोड कसा जनरेट करायचा हे दाखवतो.

[Sample Code](../../../../../../code/06.E2E/E2E_OpenVino_Phi3-vision.ipynb)

येथे टप्प्याटप्प्याने स्पष्टीकरण आहे:

1. **इम्पोर्ट्स आणि सेटअप**:
   - आवश्यक लायब्ररी आणि मॉड्यूल्स इम्पोर्ट केले जातात, ज्यात प्रतिमा प्रक्रिया करण्यासाठी `requests`, `PIL` आणि मॉडेल आणि प्रक्रिया हाताळण्यासाठी `transformers` यांचा समावेश आहे.

2. **प्रतिमा लोड करणे आणि दाखवणे**:
   - एक प्रतिमा फाइल (`demo.png`) `PIL` लायब्ररी वापरून उघडली जाते आणि दाखवली जाते.

3. **प्रॉम्प्ट परिभाषित करणे**:
   - एक मेसेज तयार केला जातो ज्यात प्रतिमा आणि Python कोड जनरेट करण्याची विनंती असते, जो प्रतिमेवर प्रक्रिया करेल आणि `plt` (matplotlib) वापरून सेव्ह करेल.

4. **प्रोसेसर लोड करणे**:
   - `AutoProcessor` pretrained मॉडेलपासून `out_dir` डायरेक्टरीतून लोड केला जातो. हा प्रोसेसर टेक्स्ट आणि प्रतिमा इनपुट्स हाताळेल.

5. **प्रॉम्प्ट तयार करणे**:
   - मॉडेलसाठी योग्य असा प्रॉम्प्ट तयार करण्यासाठी `apply_chat_template` मेथड वापरली जाते.

6. **इनपुट्स प्रक्रिया करणे**:
   - प्रॉम्प्ट आणि प्रतिमा अशा टेन्सर्समध्ये रूपांतरित केली जातात ज्यांना मॉडेल समजू शकते.

7. **जनरेशन आर्ग्युमेंट्स सेट करणे**:
   - मॉडेलच्या जनरेशन प्रक्रियेसाठी आर्ग्युमेंट्स परिभाषित केले जातात, जसे की जनरेट करावयाच्या नवीन टोकन्सची कमाल संख्या आणि आउटपुट सॅम्पल करायचा की नाही.

8. **कोड जनरेट करणे**:
   - इनपुट्स आणि जनरेशन आर्ग्युमेंट्सच्या आधारे मॉडेल Python कोड जनरेट करते. आउटपुट हाताळण्यासाठी `TextStreamer` वापरला जातो, प्रॉम्प्ट आणि स्पेशल टोकन्स वगळून.

9. **आउटपुट**:
   - जनरेट केलेला कोड प्रिंट केला जातो, ज्यात प्रतिमेवर प्रक्रिया करणारा आणि प्रॉम्प्टनुसार सेव्ह करणारा Python कोड असतो.

हा डेमो दाखवतो की pretrained मॉडेलचा वापर करून OpenVino वापरून वापरकर्त्याच्या इनपुट आणि प्रतिमा आधारित डायनॅमिक कोड कसा तयार करता येतो.

**अस्वीकरण**:  
हा दस्तऐवज AI अनुवाद सेवा [Co-op Translator](https://github.com/Azure/co-op-translator) चा वापर करून अनुवादित केला आहे. आम्ही अचूकतेसाठी प्रयत्न करतो, तरी कृपया लक्षात घ्या की स्वयंचलित अनुवादांमध्ये चुका किंवा अचूकतेच्या त्रुटी असू शकतात. मूळ दस्तऐवज त्याच्या मूळ भाषेत अधिकृत स्रोत मानला जावा. महत्त्वाच्या माहितीसाठी व्यावसायिक मानवी अनुवाद करण्याचा सल्ला दिला जातो. या अनुवादाच्या वापरामुळे होणाऱ्या कोणत्याही गैरसमजुती किंवा चुकीच्या अर्थलाभासाठी आम्ही जबाबदार नाही.