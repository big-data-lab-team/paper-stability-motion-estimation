# Jul 9

Back to experimentation. All bootstrap iterations are identical, 
presumably due to nibabel update (one voxel doesn't do anything any more).
Updated one-voxel, relaunched bootstrap experiment on Graham yesterday.

Back to writing, updated references and intro.

# Apr 23

Exceeded file quota on graham, targzing CorrBIDS-one-voxel and CorrBIDS-one-voxel-rep1
so that I can remove these repos.

Should mention that we did 2 reps of the individual algos to check for
random effects.

# 
Mention head avoidance techniques
buff.ly/2X5aJut

Do Public Datasets Assure Unbiased Comparisons for Registration Evaluation?https://arxiv.org/abs/2003.09483

On the Application of Registration uncertainty
https://link.springer.com/chapter/10.1007/978-3-030-32245-8_46

Probabilistic Image Registration via Deep Multi-class Classification: Characterizing Uncertainty
https://link.springer.com/chapter/10.1007/978-3-030-32689-0_2

# Feb 20

Launching bootstrap on NIak and niak unchained

# Feb 19

Launching bootstrap (30) on AFNI, SPM, FSL.
niak and niak unchained still running in rep2

Once we have 20 bootstrap results, we could aggregate them for first site.

Fuzzy env is ready for Niak, FSL and AFNI.
See run_mcflirt_fuzzy.sh
-> Only have to change the image there. !!! be careful to change image path in analysis.py  

# Feb 11

Discussion avec Pierre sur Slack: ne pas filtrer les FDs mais plotter 
le delta FD en fonction du FD

check_reps.py will check if results are identical between rep 1 and rep2. 
So far so good, but many results are still missing in rep2.

# Feb 6

https://www.sciencedirect.com/science/article/pii/S1053811919309917
https://www.sciencedirect.com/science/article/pii/S1053811920301658


Only niak is still running now.

Should add statistical significance and swarm plots to box plots.

# Feb 5

niak and niak_unchained are slowly finishing

Sent to Pierre:

Sinon, je me suis remis sérieusement à l'expérience one-voxel pour l'estimation du mouvement. Je voudrais faire de la QC sur les 680 datasets x 6 méthodes et pensais utiliser 3 critères :
virer les datasets où le max FD est supérieur à un seuil S1
virer les datasets où plus de x % des FDs sont supérieurs à un seuil S2
virer les datasets pour lesquels la corrélation minimale entre deux paires de méthodes est inférieure à un seuil S3
Et dans tous les cas virer les résultats obtenus pour toutes les méthodes quand une d'entre elles ne passe pas 1, 2 ou 3. Sounds good ? Si oui, as-tu une idée a-priori sur les valeurs de S1, S2, S3 ? Si non, je vais regarder les distributions, en gardant en tête la pratique usuelle de 0.2 mm utilisée quand on scrubbe.

With the current results:
* S1=4mm filters 2.8% of the datasets
* S3=0.4 sounds good
Should remove mcflirt_fudge from the study before doing QC

# Feb 4

AFNI is done, only two datasets failed because of black volume.
Several had a warning that dataset is oblique. 

mcflirt_fudge is done, no error.

spm and mcflirt are done
22 spm failed, 19 "invalid range", 2 niak unzipping errors, 1 can't open image file
mcflirt, no error

data dependence persists
same number of subjects in each site


# Feb 3

re-runing experiment from scratch, sequentially for each algo (6 jobs total)

# Jan 29

Re-computed $OVMC_BASE/CorrBIDS-one-voxel/BNU_2/sub-0025932/ses-2/func/sub-0025932_ses-2_task-rest_run-1_bold_niak_no_chained_init.fd
(in test) because it looked very weird, and got very different result (repeated twice).
Is there a race condition in niak? maybe the mounting of tmp dirs

0. Baseline estimations
    Dataset presentation, QC
1. There is uncertainty (1-voxel, 1 rep)
FSL and Niak are sensitive to one-voxel perturbations, with $\Delta FD$ in 
2. Uncertainty reduction by bootstrap (1-voxel, 30 reps)
   bootstrap variance provides a better uncertainty measure
3. Source of uncertainty (chained initialization)
4. Another approach to uncertainty measurement (MCA)

related work: bronze standard

calculer l'incertitude de l'estimation de mouvement: delta FD sur 2 reps mais aussi variance de l'echantillon bootstrap
comparer avec incertitude MCA?

calculer le Delta FD entre toolkits

Read this: https://www.sciencedirect.com/science/article/pii/S1053811917310972#fig3
refer to fmriprep https://fmriprep.readthedocs.io/en/stable/

Refer to https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3254728/

For QC, start by discarding datasets where max fd > 0.2mm

In intro, add scrubbing as a potential source of instability coming from motion correction.

Check FD computation from https://github.com/poldrack/fmriqa/blob/2b217dad803e5fc0e3ddca3d7099d99b3b1db089/compute_fd.py
looks ok

# Jan 22

yesterday's updates seem to have fixed a lot of issues. Some errors remain:
* In Niak, nifti to minc conversion sometimes seems to be produce files that minctracc refuses to process

# Jan 21

* Errors in Niak data processing are most likely coming from concurrent 
  runs, as identity.xfm seems to be written in the cwd (shared) for every task
  -> let's fix it in Niak, by only running identify.xfm if it doesn't exist 
* Checking that one-voxel perturbations are correct in list_perturbations.py
* Should run everything once again to check for random seeds / concurrency issues, etc

# Before 
* Implementation Python de l'estimation de mvt de Niak. Christian Danserau. 1 notebook pour reproduire le resultat principal. Dans Binder.
* minctracc rajoute du bruit? regarder le code. --speckle 0.
* essayer la mediane dans le bootstrap. Regarder la distribution des estimees bootstrap. 
* ajouter AFNI

* Regarder la distribution des estimees bootstrap et s'il y a des outliers.
* Measure the max FD error in the dataset
* Report it by site
* Report it by TR value
* Check how many volumes would be scrubbed before and after one voxel (Dice coefficient on the sets of volumes volumes .2mm ou .5mm de FD). Scrubbing would amplify motion estimation
errors by removing volumes.
* Check correlations between algorithms.
# Le QC est le niveau de mouvement. Pas de QC visuel. QC: compare max FD (QC fail vs QC pass). Compare the QC pass set (effect of one voxel)

* Literature:
https://www.biorxiv.org/content/early/2018/06/07/337360?%3Fcollection=
Power (see reading club)
