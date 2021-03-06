# Vue d'ensemble
## [Qu’est-ce que Site Recovery ?](site-recovery-overview.md)
## [Comment Site Recovery fonctionne-t-il ?](site-recovery-components.md)
## [Fonctionnement de la réplication Hyper-V dans Azure](site-recovery-hyper-v-azure-architecture.md)
## [Quelles charges de travail pouvez-vous protéger ?](site-recovery-workload.md)
## [Matrice de prise en charge Site Recovery](site-recovery-support-matrix-to-azure.md)
## [FORUM AUX QUESTIONS](site-recovery-faq.md)
## [Voir une présentation](https://azure.microsoft.com/resources/videos/index/?services=site-recovery)

# Prise en main
## [Réplication de machines virtuelles VMware dans Azure](site-recovery-vmware-to-azure.md)
## [Répliquer des serveurs physiques dans Azure](site-recovery-physical-servers-to-azure.md) 
## [Répliquer des machines virtuelles Hyper-V dans Azure (avec VMM)](site-recovery-vmm-to-azure.md)
## [Réplication de machines virtuelles Hyper-V dans Azure](site-recovery-hyper-v-site-to-azure.md)
## [Répliquer des machines virtuelles Hyper-V vers un site secondaire (avec VMM)](site-recovery-vmm-to-vmm.md)
## [Répliquer des machines virtuelles VMware et des serveurs physiques vers un site secondaire](site-recovery-vmware-to-vmware.md)
## [Répliquer des machines virtuelles VMware dans Azure dans le cadre d’un déploiement mutualisé (CSP)](site-recovery-multi-tenant-support-vmware-using-csp.md)

# Procédure
## Planification
### [Conditions préalables au déploiement](site-recovery-prereq.md)
### [Planifier l’infrastructure réseau](site-recovery-network-design.md)
### [Planifier la capacité et mettre à l’échelle la réplication VMware dans Azure](site-recovery-plan-capacity-vmware.md)
### [Deployment Planner pour la réplication de VMware vers Azure](site-recovery-deployment-planner.md)
### [Capacity Planner pour la réplication Hyper-V](site-recovery-capacity-planner.md)

## Configuration
### [Configurer l’environnement source](site-recovery-set-up-vmware-to-azure.md)
### [Configurer l’environnement cible](site-recovery-prepare-target-vmware-to-azure.md)
### [Configurer les paramètres de réplication](site-recovery-setup-replication-settings-vmware.md)
### [Déployer le service Mobilité pour la réplication VMware](site-recovery-vmware-to-azure-install-mob-svc.md)
#### [Déployer le service Mobilité avec System Center Configuration Manager](site-recovery-install-mobility-service-using-sccm.md)
#### [Déployer le service Mobilité avec Azure Automation DSC](site-recovery-automate-mobility-service-install.md)
### [Activer la réplication](site-recovery-replicate-vmware-to-azure.md)
## Basculement et restauration automatique
### [Basculer des machines protégées](site-recovery-failover.md)
### [Configurer des plans de récupération](site-recovery-create-recovery-plans.md)
#### [Ajouter des Runbooks Azure à des plans de récupération](site-recovery-runbook-automation.md)
### [Exécuter un test de basculement](site-recovery-test-failover-to-azure.md)
### [Restaurer la protection des machines après un basculement](site-recovery-how-to-reprotect.md)
### [Effectuer une restauration automatique à partir d’Azure](site-recovery-failback-azure-to-vmware.md)

## Migrer
### [Migration vers Azure](site-recovery-migrate-to-azure.md)
### [Migrer entre des régions Azure](site-recovery-migrate-azure-to-azure.md)
### [Migrer des instances Windows AWS dans Azure](site-recovery-migrate-aws-to-azure.md)
## Charges de travail
### [Active Directory et DNS](site-recovery-active-directory.md)
### [SQL Server](site-recovery-sql.md)
### [SharePoint](site-recovery-workload.md#protect-sharepoint)
### [Dynamics AX](site-recovery-workload.md#protect-dynamics-ax)
### [RDS](site-recovery-workload.md#protect-rds)
### [Microsoft Exchange](site-recovery-workload.md#protect-exchange)
### [SAP](site-recovery-workload.md#protect-sap)
### [Autres charges de travail](site-recovery-workload.md#workload-summary)
## Automatiser la réplication
### [Automatiser la réplication Hyper-V dans Azure (sans VMM)](site-recovery-deploy-with-powershell-resource-manager.md)
### [Automatiser la réplication Hyper-V dans Azure (avec VMM)](site-recovery-vmm-to-azure-powershell-resource-manager.md)
### [Automatiser la réplication Hyper-V vers un site secondaire (avec VMM)](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
## Gérer
### [Modifier les paramètres de réplication](site-recovery-setup-replication-settings-vmware.md#edit-replication-policy.md)
### [Gérer des serveurs de processus dans Azure](site-recovery-vmware-setup-azure-ps-resource-manager.md)
### [Gérer le serveur de configuration](site-recovery-vmware-to-azure-manage-configuration-server.md)
### [Gérer des serveurs de processus de montée en puissance parallèle](site-recovery-vmware-to-azure-manage-scaleout-process-server.md)
### [Gérer des serveurs vCenter](site-recovery-vmware-to-azure-manage-vCenter.md)
### [Supprimer des serveurs et désactiver la protection](site-recovery-manage-registration-and-protection.md)
## [Surveiller et résoudre des problèmes](site-recovery-monitoring-and-troubleshooting.md)

# Référence
## [PowerShell](/powershell/module/azurerm.siterecovery)
## [PowerShell Classic](/powershell/module/azure/?view=azuresmps-3.7.0)
## [REST](https://msdn.microsoft.com/en-us/library/mt750497)

# Rubriques connexes
## [Azure Automation](/azure/automation/)

# les ressources
## [Parcours d’apprentissage](https://azure.microsoft.com/documentation/learning-paths/site-recovery/)
## [Forum](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hypervrecovmgr)
## [Blog](http://azure.microsoft.com/blog/tag/azure-site-recovery/)
## [Tarification](https://azure.microsoft.com/pricing/details/site-recovery/)
## [Mises à jour de service](https://azure.microsoft.com/updates/?product=site-recovery)
