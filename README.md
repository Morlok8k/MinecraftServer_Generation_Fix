/*
 * This is a hack to the Minecraft Server to allow initial generation of the nether, and the end.
 * It requires a decompiled server.
 * 
 * Your Mileage may vary.  this is based on the 1.4.7 code.
 * 
 * This modifies "/net/minecraft/server/MinecraftServer.java"
 * 
 * Below you will find the original and the modified versions of "initialWorldChunkLoad()"
 * This way you can compare what i've done, and understand it.
 * 
 * Visit:
 * http://www.minecraftforum.net/topic/1378775-server-generation-fix-mod/
 * for the actual mod.
 * 
 */

    /*    //Original Version
    protected void initialWorldChunkLoad()
    {
        int var5 = 0;
        this.setUserMessage("menu.generatingTerrain");
        byte var6 = 0;
        logger.info("Preparing start region for level " + var6);
        WorldServer var7 = this.worldServers[var6];
        ChunkCoordinates var8 = var7.getSpawnPoint();
        long var9 = System.currentTimeMillis();

        for (int var11 = -192; var11 <= 192 && this.isServerRunning(); var11 += 16)
        {
            for (int var12 = -192; var12 <= 192 && this.isServerRunning(); var12 += 16)
            {
                long var13 = System.currentTimeMillis();

                if (var13 - var9 > 1000L)
                {
                    this.outputPercentRemaining("Preparing spawn area", var5 * 100 / 625);
                    var9 = var13;
                }

                ++var5;
                var7.theChunkProviderServer.loadChunk(var8.posX + var11 >> 4, var8.posZ + var12 >> 4);
            }
        }

        this.clearCurrentTask();
    }
    */

    protected void initialWorldChunkLoad()  //Morlok8k edit
    {
        logger.info("Morlok8k's Server Generation Fix Mod (Server 1.4.7) is installed.");
        WorldServer[] dimensionList = this.worldServers;
        int dimensions = dimensionList.length;

        for (int i = 0; i < dimensions; ++i)
        {
            WorldServer thisDimension = dimensionList[i];

            File file = new File(thisDimension.provider.getDimensionName());

            if (thisDimension != null && file.exists())
            {

                int var5 = 0;
                this.setUserMessage("menu.generatingTerrain");

                logger.info("Preparing start region for level \'" + thisDimension.getWorldInfo().getWorldName() + "\'/" + thisDimension.provider.getDimensionName());

                ChunkCoordinates var8 = thisDimension.getSpawnPoint();
                long var9 = System.currentTimeMillis();

                for (int var11 = -192; var11 <= 192 && this.isServerRunning(); var11 += 16)
                {
                    for (int var12 = -192; var12 <= 192 && this.isServerRunning(); var12 += 16)
                    {
                        long var13 = System.currentTimeMillis();

                        if (var13 - var9 > 1000L)
                        {
                            this.outputPercentRemaining("Preparing spawn area", var5 * 100 / 625);
                            var9 = var13;
                        }

                        ++var5;
                        thisDimension.theChunkProviderServer.loadChunk(var8.posX + var11 >> 4, var8.posZ + var12 >> 4);
                    }
                }

                this.clearCurrentTask();

            } else {
                logger.info("Skipping Generation of level \'" + thisDimension.provider.getDimensionName() + "\'.");
            }
        }
    }
