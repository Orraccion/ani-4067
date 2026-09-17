> Cours VR/XR - Exercice tableau des latences

| # | Étape du pipeline | Valeur mesurée | Détail | Source |
|---|---|---|---|---|
| 1 | Capteurs | < 3 ms (total IMU -> photon) / 1-2 ms par échantillon (500-1000Hz) | IMU Bosch BMI085 annoncée pour VR avec <3ms M2P | Bosch Sensortec via EDN / Documentation Vive Tracking |
| 2 | Transmission + Fusion Tracking | 4 ms | Latence du tracking server qui fusionne les samples capteurs. Sampling rate 250Hz = 4ms | Doc système Vive Lighthouse : "Sampling rate 250Hz, 4ms latency - Tracking server fuses sensor samples" |
| 3 | Rendu | Budget variable : 5,9ms @72Hz / 3,1ms @90Hz / 0,3ms @120Hz - Exemple réel 11,06ms | C'est le budget restant après les 8ms incompressibles. Mesuré via VrApi Logs App GPU Time | Calcul 1000/Hz et obtenu pars les logs sur <<Oculus Developer Blog VrApi Logs>> : "App=11.06ms portion is OS-measured GPU time" |
| 4 | Composition / TimeWarp / Reprojection | 1,4 ms (TimeWarp seul) - 1 à 2 ms typique | Temps GPU pour le warp de reprojection orientation tardive | Oculus ovrgpuprofiler officiel : "1.411ms of that was TimeWarp (the preempt stage)" / Littérature : "Compositor Reprojection Typical 1-2ms" |
| 5 | Affichage | 0,33ms à 0,53ms @144Hz (Index) / 1,85ms (Vive Gen1) / 0,5-2ms général | Le temps où le pixel reste allumé | Valve officiel : "pixels illuminate in just 0.33ms" + "0.330ms to 0.530ms persistence" / Article : "screens are illuminated for between 0.5 and 2ms" |