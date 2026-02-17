---
id: news-20250805-Aiping-prone-MRI
---
## Breast MRI in the Prone Position - Impact on RF-Induced Heating of Active Implantable Medical Devices (Phys. Med. Biol.)

**Aiping Yao, Tolga Goren, Bryn A. Lloyd, Carina J. Fuss, Silvia Farcito, Jing Wang, Pengfei Yang, and Niels Kuster, Physics in Medicine and Biology 2025, Volume 70, Issue 17, online 29 July 2025; doi: 10.1088/1361-6560/adf58d**

Prone (face-down) postures, commonly used in breast examinations, as well as in wrist and elbow imaging, change the induced current path in the body and, therefore, the incident electric (E-) fields to implants during magnetic resonance imaging (MRI) scans, potentially leading to significant variations from risk predictions made on the basis of supine postures. The goal of this work is to investigate the impact of prone compared to supine postures in MRI breast examination on the radiofrequency (RF-) induced heating of medical implants. 

To assess these effects, the Virtual Population (ViP) anatomical model Ella, modified with breast shapes typical of prone examinations, was posed with one or both arms raised above the head. The head was positioned either adjacent to the bore wall, typical for breast MRI examinations where the patient is prone, or centrally within the bore, common in supine positioning. The RF-induced E-fields and worst-case power depositions at breast imaging landmarks were compared with those from the supine Ella model for a representative generic pacemaker, a deep brain stimulator (DBS) implant, and cochlear implants. 

The results indicate that prone positions typical of breast examinations can lead to differences in the incident E-field and the corresponding RF-heating at 1.5 T of up to 0.6 dB for pacemakers, 1.7 dB for DBS, and 4.9 dB for cochlear implants , compared to the corresponding supine imaging landmarks. At 3.0 T, the corresponding differences in RF-heating are 0.3 dB for pacemakers, 0.2 dB for DBS, and 2.5 dB for cochlear implants.

These findings underscore that it is important to consider patient posture in RF safety assessments of implantable medical devices in the head and neck regions. To avoid underestimation of local RF exposure near these implants, a comprehensive posture-aware safety protocol should be designed and implemented during the RF safety evaluation and labeling.

{{< modal-image news-20250805-Aiping-prone-MRI.jpg >}}
{{< /modal-image >}}
*Side and top views of ViP model Ella in default supine pose, and modified for breast examination with one or both arms over the head, positioned either realistically with the breast at coil isocenter, or offset such that the torso is at isocenter similar to the supine case.*

The E-field simulations in this work to model the induced E-fields inside the human body were performed on the [Sim4Life](https://sim4life.swiss/) platform (ZMT Zurich MedTech AG) with the EM-FDTD solver. The high-resolution [ViP](https://itis.swiss/virtual-population/virtual-population/overview/) model Ella was modified by morphing to represent MRI postures with one (“Superman”) or both arms (“Hands_Over”) up; the resulting field distributions are being added to the IT’IS [MRIxViP exposure library](https://itis.swiss/virtual-population/explib/overview/). The tissue properties were assigned according to the IT’IS [tissue properties database](https://itis.swiss/virtual-population/tissue-properties/overview/).

The scientific and technical impact of the study can be summarized as:

* Changes in patient posture, including tissue deformation (e.g., hanging breast) and positioning (e.g., face-down MRI examinations with hands over head) alter the induced field distribution, leading to a difference of >1 dB in in vivo incident E-field and power deposition for DBS and pacemaker implants, and up to 5 dB for cochlear implants
* The potential impact for cochlear implants is notably greater because changes in arm posture significantly affect the local field distribution near the ears, enhancements that are dependent on the implant, the location of the implant, and on patient anatomy but are not represented in supine evaluations
* At 1.5 T (64 MHz), anatomical positioning relative to the birdcage coil axis significantly affects exposure, with Etan and specific absorption rate values reduced by over 50% when models are moved to the iso-center – highlighting strong position-dependent coupling at lower frequencies, a phenomenon that is less pronounced at 3.0 T (128 MHz)
* For implants present during face-down MRI, posture-dependent risk must be analyzed or included in uncertainty estimates, as worst-case anatomical scenarios cannot be reliably predicted, warranting added safety margins when labeling lacks clarity

[ACCESS ARTICLE ONLINE](https://iopscience.iop.org/article/10.1088/1361-6560/adf58d)


