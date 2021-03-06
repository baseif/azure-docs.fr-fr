1. Retournez dans le Gestionnaire du cluster de basculement.  Développez les **Rôles** , puis mettez votre groupe de disponibilité en surbrillance.  Dans l'onglet **Ressources** , cliquez avec le bouton droit sur le nom de l'écouteur, puis cliquez sur Propriétés.
2. Cliquez sur l'onglet **Dépendances** . Si plusieurs ressources sont répertoriées, vérifiez que les adresses IP ont des dépendances OR (et non des dépendances AND).  Cliquez sur **OK**.
3. Cliquez avec le bouton droit sur l'écouteur, puis cliquez sur **Mettre en ligne**.
4. Une fois l’écouteur en ligne, allez dans l’onglet **Ressources**, cliquez avec le bouton droit sur le groupe de disponibilité, puis cliquez sur **Propriétés**.
   
    ![Configurer la ressource du groupe de disponibilité](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678772.gif)
5. Créez une dépendance sur la ressource de nom d'écouteur (pas le nom de ressources Adresse IP). Cliquez sur **OK**.
   
    ![Ajouter une dépendance pour le nom de l'écouteur](./media/virtual-machines-sql-server-configure-alwayson-availability-group-listener/IC678773.gif)
6. Lancez **SQL Server Management Studio** et connectez-vous au réplica principal.
7. Accédez à **Haute disponibilité AlwaysOn** | **Groupes de disponibilité** | **<AvailabilityGroupName>** | **Écouteurs de groupe de disponibilité**. 
8. Vous devez maintenant voir le nom de l'écouteur que vous avez créé dans le Gestionnaire du cluster de basculement. Cliquez avec le bouton droit sur l’écouteur, puis cliquez sur **Propriétés**.
9. Dans la zone **Port**, spécifiez le numéro de port pour l’écouteur du groupe de disponibilité à l’aide de l’élément $EndpointPort utilisé précédemment (dans ce didacticiel, la valeur par défaut était 1433), puis cliquez sur **OK**.

