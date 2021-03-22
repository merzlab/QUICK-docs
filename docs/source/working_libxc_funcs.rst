A List of Available DFT Functionals
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Based on our tests, following functionals are successully working in QUICK-21.03 serial, MPI, CUDA and CUDAMPI versions. 
Note that here we are reporting LIBXC keywords that will be specified in QUICK input. `Please use LIBXC functionals page
for more information on each keyword. <https://www.tddft.org/programs/libxc/functionals/previous/libxc-4.0.4/>`_. 

1) LDA exchange functionals
***************************

.. code-block:: none

  1. LDA_X
  2. LDA_X_2D
  3. LDA_X_RAE
  4. LDA_X_REL  


2) LDA correlation funcationals
*******************************

.. code-block:: none

  1.  LDA_C_1D_LOOS
  2.  LDA_C_2D_AMGB
  3.  LDA_C_2D_PRM
  4.  LDA_C_BR78
  5.  LDA_C_CHACHIYO
  6.  LDA_C_GK72
  7.  LDA_C_GL
  8.  LDA_C_GOMBAS
  9.  LDA_C_HL
  10. LDA_C_KARASIEV
  11. LDA_C_MCWEENY
  12. LDA_C_ML1
  13. LDA_C_ML2
  14. LDA_C_OB_PW
  15. LDA_C_OB_PZ
  16. LDA_C_OW
  17. LDA_C_OW_LYP
  18. LDA_C_PK09
  19. LDA_C_PW
  20. LDA_C_PW_MOD
  21. LDA_C_PW_RPA
  22. LDA_C_PZ
  23. LDA_C_PZ_MOD
  24. LDA_C_RC04
  25. LDA_C_RPA
  26. LDA_C_VBH
  27. LDA_C_VWN
  28. LDA_C_VWN_1
  29. LDA_C_VWN_2
  30. LDA_C_VWN_3
  31. LDA_C_VWN_4
  32. LDA_C_VWN_RPA
  33. LDA_C_WIGNER
  34. LDA_C_XALPHA


3) LDA exchange correlation functionals
***************************************

.. code-block:: none

  1.  LDA_XC_KSDT
  2.  LDA_XC_LP_A
  3.  LDA_XC_LP_B
  4.  LDA_XC_TETER93
  5.  LDA_XC_ZLP


4) GGA exchange functionals
***************************

.. code-block:: none

  1.  GGA_X_2D_B86
  2.  GGA_X_2D_B86_MGC
  3.  GGA_X_2D_B88
  4.  GGA_X_2D_PBE
  5.  GGA_X_AIRY
  6.  GGA_X_AK13
  7.  GGA_X_APBE
  8.  GGA_X_B86
  9.  GGA_X_B86_R
  10. GGA_X_B88
  11. GGA_X_BCGP
  12. GGA_X_BEEFVDW
  13. GGA_X_BPCCAC
  14. GGA_X_C09X
  15. GGA_X_CAP
  16. GGA_X_CHACHIYO
  17. GGA_X_DK87_R1
  18. GGA_X_DK87_R2
  19. GGA_X_EB88
  20. GGA_X_EV93
  21. GGA_X_G96
  22. GGA_X_HCTH_A
  23. GGA_X_HTBS
  24. GGA_X_LAG
  25. GGA_X_LAMBDA_CH_N
  26. GGA_X_LAMBDA_LO_N
  27. GGA_X_LAMBDA_OC2_N
  28. GGA_X_LG93
  29. GGA_X_LV_RPW86
  30. GGA_X_MB88
  31. GGA_X_MPBE
  32. GGA_X_MPW91
  33. GGA_X_PBE
  34. GGA_X_PBEFE
  35. GGA_X_PBEINT
  36. GGA_X_PBEK1_VDW
  37. GGA_X_PBE_R
  38. GGA_X_PBETRANS
  39. GGA_X_PW86
  40. GGA_X_PW91
  41. GGA_X_Q2D
  42. GGA_X_RGE2
  43. GGA_X_RPBE
  44. GGA_X_RPW86
  45. GGA_X_SG4
  46. GGA_X_VMT84_GE
  48. GGA_X_VMT84_PBE
  49. GGA_X_VMT_GE
  50. GGA_X_VMT_PBE
  51. GGA_X_WC
  52. GGA_X_XPBE


5) GGA correlation functionals
******************************

.. code-block:: none

  1.  GGA_C_AM05
  2.  GGA_C_APBE
  3.  GGA_C_BCGP
  4.  GGA_C_BMK
  5.  GGA_C_CS1
  6.  GGA_C_GAM
  7.  GGA_C_GAPC
  8.  GGA_C_GAPLOC
  9.  GGA_C_HCTH_A
  10. GGA_C_HYB_TAU_HCTH
  11. GGA_C_LYP
  12. GGA_C_N12
  13. GGA_C_N12_SX
  14. GGA_C_OP_B88
  15. GGA_C_OP_G96
  16. GGA_C_OP_PBE
  17. GGA_C_OP_XALPHA
  18. GGA_C_P86
  19. GGA_C_PBE
  20. GGA_C_PBEFE
  21. GGA_C_PBEINT
  22. GGA_C_PBE_JRGX
  23. GGA_C_PBELOC
  24. GGA_C_PBE_MOL
  25. GGA_C_PBE_SOL
  26. GGA_C_PW91
  27. GGA_C_Q2D
  28. GGA_C_REGTPSS
  29. GGA_C_REVTCA
  30. GGA_C_RGE2
  31. GGA_C_SCAN_E0
  32. GGA_C_SG4
  33. GGA_C_SOGGA11
  34. GGA_C_SOGGA11_X
  35. GGA_C_SPBE
  36. GGA_C_TAU_HCTH
  37. GGA_C_TCA
  38. GGA_C_TM_LYP
  39. GGA_C_TM_PBE
  40. GGA_C_W94
  41. GGA_C_WI
  42. GGA_C_WI0
  43. GGA_C_WL
  44. GGA_C_XPBE
  45. GGA_C_ZPBEINT
  46. GGA_C_ZPBESOL
  48. GGA_C_ZVPBEINT
  49. GGA_C_ZVPBESOL


6) Hybrid-GGA functionals
*************************

.. code-block:: none

  1.  HYB_GGA_XC_B1LYP
  2.  HYB_GGA_XC_B1PW91
  3.  HYB_GGA_XC_B1WC
  4.  HYB_GGA_XC_B3LYP
  5.  HYB_GGA_XC_B3P86
  6.  HYB_GGA_XC_B3PW91
  7.  HYB_GGA_XC_BHANDH
  8.  HYB_GGA_XC_MB3LYP_RC04
  9.  HYB_GGA_XC_MPW1K
  10. HYB_GGA_XC_MPW1LYP
  11. HYB_GGA_XC_MPW1PBE
  12. HYB_GGA_XC_MPW1PW
  13. HYB_GGA_XC_MPW3LYP
  14. HYB_GGA_XC_MPW3PW
  15. HYB_GGA_XC_MPWLYP1M
  16. HYB_GGA_XC_PBEH
  17. HYB_GGA_XC_PBE_MOL0
  18. HYB_GGA_XC_PBE_SOL0
  19. HYB_GGA_XC_REVB3LYP
  20. HYB_GGA_XC_X3LYP

*Last updated by Madu Manathunga on 03/20/2021.*
