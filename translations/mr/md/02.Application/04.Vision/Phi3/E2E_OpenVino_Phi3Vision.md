हे डेमो दाखवतो की पूर्वप्रशिक्षित मॉडेल वापरून प्रतिमा आणि मजकूर सूचनेच्या आधारे Python कोड कसा तयार करता येतो.

[नमुना कोड](../../../../../../code/06.E2E/E2E_OpenVino_Phi3-vision.ipynb)

येथे टप्प्याटप्प्याने स्पष्टीकरण दिले आहे:

1. **आयात आणि सेटअप**:
   - आवश्यक लायब्ररी आणि मॉड्यूल आयात केले जातात, ज्यामध्ये प्रतिमा प्रक्रिया करण्यासाठी `requests`, `PIL` आणि मॉडेल हाताळण्यासाठी व प्रक्रिया करण्यासाठी `transformers` यांचा समावेश आहे.

2. **प्रतिमा लोड करणे आणि दाखवणे**:
   - प्रतिमा फाईल (`demo.png`) `PIL` लायब्ररीचा वापर करून उघडली जाते आणि दाखवली जाते.

3. **सूचना निश्चित करणे**:
   - एक संदेश तयार केला जातो ज्यामध्ये प्रतिमा आणि ती प्रक्रिया करण्यासाठी Python कोड तयार करून `plt` (matplotlib) वापरून जतन करण्याची विनंती असते.

4. **प्रोसेसर लोड करणे**:
   - `AutoProcessor` पूर्वप्रशिक्षित मॉडेलमधून लोड केला जातो, जो `out_dir` डिरेक्टरीमध्ये निर्दिष्ट केलेला असतो. हा प्रोसेसर मजकूर आणि प्रतिमा इनपुट हाताळतो.

5. **सूचना तयार करणे**:
   - `apply_chat_template` पद्धतीचा वापर करून संदेश मॉडेलसाठी योग्य स्वरूपाच्या सूचनेत रूपांतरित केला जातो.

6. **इनपुट्स प्रक्रिया करणे**:
   - सूचना आणि प्रतिमा टेन्सर्समध्ये रूपांतरित केल्या जातात, जे मॉडेलला समजतील.

7. **जेनरेशन आर्ग्युमेंट्स सेट करणे**:
   - मॉडेलच्या जेनरेशन प्रक्रियेसाठी आर्ग्युमेंट्स निश्चित केली जातात, जसे की जास्तीत जास्त नवीन टोकन्सची संख्या आणि आउटपुट सॅम्पल करायचे की नाही.

8. **कोड तयार करणे**:
   - मॉडेल इनपुट्स आणि जेनरेशन आर्ग्युमेंट्सच्या आधारे Python कोड तयार करते. `TextStreamer` आउटपुट हाताळण्यासाठी वापरले जाते, ज्यामध्ये सूचना आणि विशेष टोकन्स वगळले जातात.

9. **आउटपुट**:
   - तयार केलेला कोड प्रिंट केला जातो, ज्यामध्ये प्रतिमा प्रक्रिया करून ती जतन करण्यासाठी Python कोड असतो, जसे सूचनेत नमूद केले आहे.

हा डेमो दाखवतो की OpenVino वापरून पूर्वप्रशिक्षित मॉडेल कसे वापरता येते, जेणेकरून वापरकर्त्याच्या इनपुट आणि प्रतिमांच्या आधारे डायनॅमिक कोड तयार करता येईल.

**अस्वीकरण**:  
हा दस्तऐवज मशीन-आधारित AI भाषांतर सेवांचा वापर करून अनुवादित करण्यात आला आहे. आम्ही अचूकतेसाठी प्रयत्नशील असलो तरी, कृपया लक्षात घ्या की स्वयंचलित भाषांतरांमध्ये चुका किंवा अचूकतेचा अभाव असू शकतो. मूळ भाषेतील मूळ दस्तऐवज हा अधिकृत स्रोत मानला जावा. महत्त्वाच्या माहितीसाठी, व्यावसायिक मानवी भाषांतराची शिफारस केली जाते. या भाषांतराचा वापर करून उद्भवलेल्या कोणत्याही गैरसमजुतींसाठी किंवा चुकीच्या अर्थासाठी आम्ही जबाबदार राहणार नाही.