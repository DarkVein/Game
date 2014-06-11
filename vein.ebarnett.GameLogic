package me.ebarnett;

import java.util.ArrayList;
import java.util.List;
import java.util.Random;
import java.util.logging.Logger;
import org.bukkit.Bukkit;
import org.bukkit.ChatColor;
import org.bukkit.GameMode;
import org.bukkit.Material;
import org.bukkit.block.Sign;
import org.bukkit.command.Command;
import org.bukkit.command.CommandSender;
import org.bukkit.entity.Player;
import org.bukkit.event.EventHandler;
import org.bukkit.event.Listener;
import org.bukkit.event.block.Action;
import org.bukkit.event.block.SignChangeEvent;
import org.bukkit.event.player.PlayerDropItemEvent;
import org.bukkit.event.player.PlayerEvent;
import org.bukkit.event.player.PlayerInteractEvent;
import org.bukkit.event.player.PlayerMoveEvent;
import org.bukkit.event.player.PlayerQuitEvent;
import org.bukkit.inventory.ItemStack;
import org.bukkit.plugin.java.JavaPlugin;
import org.bukkit.scheduler.BukkitScheduler;

public class GameLogic extends JavaPlugin implements Listener {

	private Random random = new Random(); 
	
	public Logger logger = Logger.getLogger("Minecraft");
	
	public int RewardLevel = 0;
	
	List<String> arena = new ArrayList<String>();
	List<String> frozen = new ArrayList<String>();
	
	public int num = 1;
	
	public void onEnable() {
        Bukkit.getServer().getPluginManager().registerEvents(this, this);
		logger.info("[Game] Enabled");
	}
	
	public void onDisable() {
		logger.info("[Game] Disabled");
	}
	
	@EventHandler
	public void onSignCreate(SignChangeEvent sign) {
		Player player = (Player) sign.getPlayer();
		if(sign.getLine(0).equalsIgnoreCase("Join Arena")) {
			
			player.sendMessage(ChatColor.GREEN + " [Game] You created a new Join sign");
			sign.setLine(2, ChatColor.BOLD + "" + player.getName());
			
		}
	}
	
	@EventHandler
	public void onFlickLever(PlayerInteractEvent e) {
		Player player = (Player) e.getPlayer();
		if (e.getAction().equals(Action.RIGHT_CLICK_BLOCK) && arena.contains(player.getName())) {
			if(e.getClickedBlock().getType() == Material.SIGN || e.getClickedBlock().getType() == Material.SIGN_POST || e.getClickedBlock().getType() == Material.WALL_SIGN) {
				Sign sign = (Sign) e.getClickedBlock().getState();
				if(sign.getLine(0).equalsIgnoreCase("Join Arena")) {
					RandomChoice(player);
			} 
			} else if(e.getClickedBlock().getType() == Material.CHEST 
					|| e.getClickedBlock().getType() == Material.ENDER_CHEST
					|| e.getClickedBlock().getType() == Material.TRAPPED_CHEST) {
				arena.remove(player.getName());
				player.closeInventory();
				ArenaDeath(player);
				int chest_locY = e.getClickedBlock().getLocation().getBlockY();
				int chest_locX = e.getClickedBlock().getLocation().getBlockX();
				int chest_locZ = e.getClickedBlock().getLocation().getBlockZ();
				CancelOutChest(chest_locY,chest_locX,chest_locZ, player);
				e.getClickedBlock().setType(Material.AIR);
				frozen.add(player.getName());
				player.sendMessage(ChatColor.RED + "Don't cheat please, k?");
			}
		}
	}
	
	@SuppressWarnings("deprecation")
	public void CancelOutChest(int Y, int X, int Z, Player player) {
		player.setHealth(0);
		Bukkit.broadcastMessage(ChatColor.BOLD + "   [EXPLOIT] " + ChatColor.RED + player.getName() + " &lChest Cheat Attempt: Y: " + Y + " X: " + X + " Z: " + Z);
		if(player.getGameMode().equals(GameMode.CREATIVE)) {
			player.setGameMode(GameMode.ADVENTURE);
			player.closeInventory();
		}
	}
	
	  @EventHandler
	  public void onPlayerMove(PlayerMoveEvent e) {
		 if(frozen.contains(e.getPlayer().getName())) {
			  e.setCancelled(true);
			  Frozen();
		  }
	  }
	
	  public void Frozen() {
	        BukkitScheduler scheduler = Bukkit.getServer().getScheduler();
	        scheduler.scheduleSyncRepeatingTask(this, new Runnable() {
	            @Override
	            public void run() {
	                if(num == 1) {
	                	num++;
	                } else if (num == 2) {
	                	num++;
	                } else if (num == 3) {
	                	num++;
	                } else if(num == 4) {
	                	frozen.clear();
	                	num = 1;
	                }
	            }
	        }, 100L, 20 * 20);
	  }
	  
	public void RewardLevel(Player player) {
		RewardLevel++;
		if(RewardLevel == 1) {
			player.getInventory().addItem(new ItemStack(Material.APPLE));
			player.updateInventory();
		} else if (RewardLevel == 2) {
			player.getInventory().addItem(new ItemStack(Material.FLINT));
			player.updateInventory();
		} else if (RewardLevel == 3) {
			player.getInventory().addItem(new ItemStack(Material.GOLD_INGOT));
			player.updateInventory();
		} else if (RewardLevel == 4) {
			player.getInventory().addItem(new ItemStack(Material.DIAMOND));
			player.updateInventory();
		} else if (RewardLevel == 5) {
			player.getInventory().addItem(new ItemStack(Material.NETHER_STAR));
			player.updateInventory();
			player.sendMessage(ChatColor.BLUE + "[Game] This is the highest level. Congratulations");
			arena.remove(player.getName());
		}
	}
	
	public void RandomBadDecision(Player player) {
		String output = null;
		switch (this.random.nextInt(3)) {
		case 0:
			output = ChatColor.RED + "Die by Spider!";
			arena.remove(player.getName());
			RewardLevel = 1;
			break;
		case 1:
			output = ChatColor.RED + "Die by Endermen";
			arena.remove(player.getName());
			RewardLevel = 1;
			break;
		case 2:
			output = ChatColor.RED + "Die by Mystery";
			arena.remove(player.getName());
			RewardLevel = 1;
			break;
		}
        if (output != null) { // Just in case
            player.sendMessage(output);
        }
	}
	
	public void RandomChoice(Player player) {
        String output = null;
        switch (this.random.nextInt(2)) {
            case 0:
                output = ChatColor.RED + "Congratulations, You survived.";
                RewardLevel(player);
                RewardLevel++;
                break;
            case 1:
                output = ChatColor.GOLD + "Bad choice.";
                RewardLevel = 0;
                RandomBadDecision(player);
                ArenaDeath(player);
                break;
        }
        if (output != null) { // Just in case
            player.sendMessage(output);
        }
	}
	
	public void ArenaDeath(Player player) {
		if (arena.contains(player.getName())) {
			player.getInventory().clear();
			Bukkit.broadcastMessage(ChatColor.RED + player.getName() + " Took their chances and lost!");
			arena.remove(player.getName());
		}
	}
	
	@EventHandler
	public void onDropItems(PlayerDropItemEvent e) {
		if (arena.contains(e.getPlayer().getName())) {
			e.setCancelled(true);
		}
	}
	
	@EventHandler
	public void onPlayerDisconnect(PlayerQuitEvent e) {
		if(arena.contains(e.getPlayer().getName())) {
		arena.remove(e.getPlayer().getName());
		}
	}
	
	public boolean onCommand(CommandSender sender, Command command, String label, String[] args) {
    	Player player = (Player) sender;
	    if (label.equalsIgnoreCase("flip")) {
	        if (sender instanceof Player) {
	        	RandomChoice(player);
	        } else {
	            sender.sendMessage("This command can only be used by a player.");
	        }
	        return true;
	    } else if(label.equalsIgnoreCase("join")) {
	    	if(arena.contains(player.getName())) {
	    		player.sendMessage(ChatColor.RED + "You are already in the game");
	    	} else {
	    	arena.add(player.getName());
	    	player.sendMessage(ChatColor.BLUE + "Added to the Arena");
	    	}
	    } else if(label.equalsIgnoreCase("leave")) {
	    	if(arena.contains(player.getName())) {
	    	arena.remove(player.getName());
	    	player.sendMessage(ChatColor.YELLOW + "Left the arena");
	    	} else {
	    		player.sendMessage(ChatColor.RED + "Not in Game.");
	    	}
	    }
	    return false;

	}


}
